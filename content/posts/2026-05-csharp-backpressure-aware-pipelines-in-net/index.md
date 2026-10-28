+++
title = "C# Channels - Why so forgotten? Designing High-Throughput Background Pipelines"
date = 2026-05-13T10:00:00-03:00
description = "Learn how to build high-throughput background pipelines in ASP.NET Core using System.Threading.Channels, backpressure, batching and Background Services."
draft = false
slug = "csharp-backpressure-aware-pipelines-in-net"
tags = [".net", "csharp", "aspnetcore", "performance", "concurrency", "channels"]
categories = ["Backend", "Advanced C#"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Diagram illustrating a high-throughput background pipeline using System.Threading.Channels in ASP.NET Core"
    caption = "Use bounded channels, backpressure, and batching to keep APIs responsive under load"
    relative = true
+++

# Designing high-throughput background pipelines with C# System.Threading.Channels

When our project receives thousands of requests per minute, we need to take some cares.
Usually, the bottleneck isn't CPU.

Probabily the problem is one (or more) of these: I/O contention, thread starvation, uncontrolled concurrency, excessive allocations... et cetera.

One of the most common mistakes in high-throughput (even in medium-throughput) APIs is coupling request processing with expensive secondary operations such as:
- audit logging;
- telemetry;
- webhook dispatching;
- email sending;
- analytics;
- event persistence.

So it causes response times increasing, thread pressure growing and throughput collapses.

This is exactly the kind of problem `System.Threading.Channels` helps to deal with.

---

# What are Channels?

They work on this concepts: producers, that produce (no way!) data and the consumers (surprise) consume, in an asynchronous way.
And Channels coordinate flow control between both sides.

To make it easier to understand: it behaves like an in-memory asynchronous queue:

```text
[ Producers ] ---> ( Channel / Buffer ) ---> [ Consumers ]
```

Different from traditional collections, Channels were designed around:
- async-first APIs;
- low-allocation patterns;
- backpressure control;
- lock minimization;
- efficient coordination between concurrent workloads.

---

# Ok, we have ConcurrentQueue for that

YES, but: `ConcurrentQueue<T>` solves thread safety.

It **doesn't** solve asynchronous coordination.

Consumers still need polling strategies, like timers, busy waiting, manual signaling, semaphores (we have a post on this blog about), custom synchronization logic.

Channels solve both, concurrency safety and asynchronous coordination.

This removes unnecessary thread usage and significantly reduces contention under load.

---

# Backpressure matters (a lot)

One of the most dangerous characteristics of internal pipelines is unbounded growth.

If producers generate work faster than consumers can process it, memory usage grows indefinitely.

Bounded channels allow configurable backpressure policies.

```csharp
using System.Threading.Channels;

var options = new BoundedChannelOptions(1000)
{
    FullMode = BoundedChannelFullMode.Wait,
    SingleReader = true, // This line makes just one consumer capable of read the channel
    SingleWriter = false
};

var channel = Channel.CreateBounded<AuditEvent>(options);
```

When the queue reaches capacity, you explicitly define what happens:
- `Wait`: to hold the flow.
- `DropNewest`: to drop the new items.
- `DropOldest`: to drop the items that are waiting longer.
- `DropWrite`

So, on this way, we know the behavior that is expected.
(this post is getting bigger then I liked, but let's keep going, the subject is worth it)

---

# Why do Channels perform so well?

Most Channel operations internally use `ValueTask` (a struct) instead of `Task` on synchronous operations.

Why does this matter?

Because asynchronous coordination happens constantly in high-throughput systems.

Avoiding unnecessary heap allocations, helping at GC pressure.

This becomes especially important under sustained load.

Channels are also heavily optimized for:
- low-lock coordination;
- cache-friendly access patterns;
- asynchronous signaling;
- throughput-oriented workloads;

---

# A real-world scenario

The last project I worked with, I needed to register all authenticated user actions/requests.

If I insert a SQL operation together the user interaction, it'd increase response latency dramatically.

Instead:
1. requests enqueue lightweight events into a bounded channel.
2. background workers process events asynchronously.
3. batching strategies reduce I/O overhead (here background services play together)

The API remains responsive while background throughput stays controlled.

Could I use a fire-and-forget?
Yes, I could.
Should I?
Absolutely not!

---

# Writing into the Channel

```csharp
public interface IAuditEventQueue
{
    ValueTask EnqueueAsync(AuditEvent auditEvent);
}

public class AuditEventQueue : IAuditEventQueue
{
    private readonly Channel<AuditEvent> _channel;

    public AuditEventQueue()
    {
        var options = new BoundedChannelOptions(1000)
        {
            FullMode = BoundedChannelFullMode.Wait,
            SingleReader = true,
            SingleWriter = false
        };

        _channel = Channel.CreateBounded<AuditEvent>(options);
    }

    public ValueTask EnqueueAsync(AuditEvent auditEvent)
        => _channel.Writer.WriteAsync(auditEvent);

    public ChannelReader<AuditEvent> Reader => _channel.Reader;
}
```

Inside the API:

```csharp
await _queue.EnqueueAsync(new AuditEvent
{
    User = request.User.Id,
    RequestData = request.Data,
    CreatedAt = DateTime.UtcNow
});
```

The request completes quickly without waiting for expensive database/log operations.

---

# BackgroundService combo!

Channels become especially powerful when work with `BackgroundService`.
I'd even say they are almost siamese twins.
(ok, maybe I'm goint too far)

```csharp
public class AuditEventWorker : BackgroundService
{
    private readonly AuditEventQueue _queue;
    private readonly IAuditRepository _repository;

    public AuditEventWorker(
        AuditEventQueue queue,
        IAuditRepository repository)
    {
        _queue = queue;
        _repository = repository;
    }

    protected override async Task ExecuteAsync(
        CancellationToken stoppingToken)
    {
        while (await _queue.Reader.WaitToReadAsync(stoppingToken))
        {
            var batch = new List<AuditEvent>();

            while (
                batch.Count < 50 &&
                _queue.Reader.TryRead(out var auditEvent))
            {
                batch.Add(auditEvent);
            }

            if (batch.Count > 0)
            {
                await _repository.BulkInsertAsync(batch);
            }
        }
    }
}
```

---

# Don't forget to complete the Channel

When the producer is done writing, it must signal the channel that no more data will come:

```csharp
_channel.Writer.Complete();
```

If you don't call this, `WaitToReadAsync` will never return `false`, and your `BackgroundService` will hang indefinitely waiting for data that will never arrive.

Considere this pattern to complete the channel on application shutdown:

```csharp
public async ValueTask DisposeAsync()
{
    _channel.Writer.Complete();
    await _channel.Reader.Completion;
}
```

`Reader.Completion` is a `Task` that completes when the channel is fully drained — useful to ensure no events are lost during graceful shutdown.

---

# Why is batching important

Batching is one of the most important throughput optimizations in distributed systems.

With no batching, each database write is a round trip.

If you batch 50 or 100 events, you just 'go to' the database server once. But be careful with oversized batches, because Channels are 'in memory', if the process crashes, you lost the data. Try to set a timer to deal with the batch, even it has fewer items you planned.

It reduces:
- network round trips;
- transaction overhead;
- database lock contention.

---

# ReadAllAsync vs WaitToReadAsync

For simple sequential processing, `ReadAllAsync` is usually enough.

```csharp
await foreach (var item in reader.ReadAllAsync(stoppingToken))
{
    await ProcessAsync(item);
}
```

However, when you need:
- batching;
- buffering;
- throughput control;
- partial draining.

`WaitToReadAsync` + `TryRead` provides much more flexibility.

---

# Keep an eye on your Channel

Bounded channels have a `Count` property that tells us how many items are currently buffered:

```csharp
var pending = _channel.Reader.Count;
```

In production, this is valuable. If `Count` is consistently near your capacity limit, it means consumers aren't keeping up - a sign you need more workers, larger batches, or a faster consumer implementation.

You can expose this as a metric:

```csharp
// Example with a simple health check
if (_channel.Reader.Count > _options.Capacity * 0.8)
{
    _logger.LogWarning("Channel pressure detected: {Count} items pending",
        _channel.Reader.Count);
}
```

Treating channel pressure as an observable signal turns a silent failure mode into an actionable alert.

---

# When aren't Channels enough

Channels are in-memory primitives.

So, if you wait a lot to deal with the messages (example: you configured a big batch), and the process crashes, you'll lost your messages.

At the case I mentioned before (Audit), I have a 50 messages batch size... BUT, each 10 seconds, the messages are processed, even they are fewer then 50.

This makes Channels ideal for:
- internal pipelines;
- transient workloads;
- performance-sensitive coordination.

But, when you need cross-service messaging, guaranteed delivery, et cetera, Channels aren't your guy.

For those scenarios, technologies such as:
- Kafka;
- RabbitMQ;
- Azure Service Bus;
- AWS SQS.
are more appropriate.

---

# That's it

`System.Threading.Channels` is one of the most underrated (forgotten, in fact) concurrency primitives in modern .NET.

It provides:
- async-first coordination;
- efficient backpressure handling;
- low allocation overhead;
- clean producer/consumer semantics.

For many cases, they can eliminate the need for external queues entirely.

---

*I also wrote about this topic for [Balta.io](https://blog.balta.io/channels-em-c-como-evitar-gargalos-em-apis-de-alto-volume/) - one of the main .NET references in Brazil - covering the same concepts with a more introductory approach, in Portuguese. If you know someone learning .NET, it might be a good starting point.*
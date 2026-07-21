+++
title = "Beyond LLM: models are what really matter!"
date = 2026-06-15T10:00:00-03:00
description = "Stop using LLMs for everything. A practical guide to the AI model landscape: embeddings, reranking, vision, image generation, and when each one beats an LLM."
draft = false
slug = "ai-beyond-llms-know-models"
tags = ["AI", "LLM", "Embeddings", "Vector", "RAG", "Ollama", "Machine Learning"]
categories = ["AI", "Software Architecture"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Four trophies representing the main AI model types: Embeddings, Vision Models, Image Generation and LLM"
    caption = "Each model does its job - pick the right one for the task"
    relative = true
+++

## Stop Using LLMs for Everything

When we think about AI, the first thought is: LLMs. Chatting with the machine is cool, and the user loves it.

BUT it's not enough, you don't need a robot friend (I hope, at least).
To an engineer, not even 'AI skills' are enough... we need to go deeper.

And one of the most important (and forgotten) subjects is: models.

Each one is focused on a specific situation.

In this post I'm going to describe the main kinds of models and some models I used or you can use. And how to try them locally (spoiler: use Ollama).

First, the star, the one the people love:

---

## LLM

I won't go too deep on these ones, because you can find lots of information about them.
The stars: OpenAI GPT, Google DeepMind Gemini, Anthropic Claude, etc.
You can use these with the official API.

Check it out:
There are many smaller models that you can run locally.

- **Qwen3** - general processing and coding (available in 4B, 8B, 14B, 30B+).
- **Gemma 3** - great cost-to-benefit relation and supports multimodal (text, image and audio).
- **Llama** - very popular and well-supported.

### Why consider running local LLMs?

You can save money on simple tasks, such as data extraction. For example, if you have a description of a product and need to extract its attributes into a json, Llama 4 can help you a lot.

---

## Vector/embedding

The next is a very useful one, this model rules others! Vector/embedding models convert your human text into vectors, and it can be compared by similarity... when you write 'happy', it's related to 'smile', 'party', etc.

They are the most important ones on a RAG (Retrieval-Augmented Generation) system.

These models, by themselves, don't do the searching, they generate the math that allows your Vector Database to find similarities.

Check some of them:

- **bge-m3** - powerful for production environments. M3 means: Multi-lingual (when you need to process text in Portuguese or Spanish alongside English), Multi-functional (supports both dense and sparse retrieval - this last one helps to find exact terms, like serial numbers or specific product IDs), and Multi-granularity (handles long contexts up to 8k tokens).
- **nomic-embed-text** - a good lighter option, which supports mainly English. But there is a new version trained in 100+ languages and MoE (embed-text-v2-moe).

### Locally or API?

To be honest, in this case, cost is rarely the deciding factor. Both API and local options are cheap compared to LLMs.

*Some API vector models*: OpenAI (text-embedding-3-small and text-embedding-3-large), Google (text-embedding-004), Cohere (embed-v4.0 - supports images), etc.

And yet, running locally on a VM or dedicated server also gives you predictable costs (no billing surprises).
But the important points are: latency (using batch processing) and privacy (you don't send your data to outside the company).

---

## ReRanking

Trusting just on vectors to choose the best option usually is, at least, dangerous. They don't have context, it's just math, pure and simple.

Our lives would turn easier just asking LLMs to rerank the best 5 items that were returned by embedding models. But it's an absolute waste of money.

Reranking models do it cheaper and fast for this kind of work.

My personal experience with these ones is just conceptual, they are very effective on general ranking, I've needed a very specific classification (Brazilian rural commerce), a small LLM was the right choice.

Some of these models:

- **bge-reranker-v2-m3** - naturally works well with bge-m3, multilingual, good in production.
- **ms-marco-MiniLM** - popular, light and frequent in benchmarks.
- **Cohere Rerank** - dedicated API, easy to work, interesting option to those who already use Cohere to embedding.

---

## Vision

This subject deserves an individual post, so I'm going to show the most interesting ones (on my view).

We can categorize them by types of return.

### Vision-Language (VLMs)

The most common one, you send a base64 image and a prompt, it returns you a text, what you asked in the prompt. Basically, they are LLMs with eyes.

Some models:

- **LLaVA** - the most popular local option (via Ollama). Available in 7B, 13B and 34B.
- **Moondream** - extremely lightweight (~2B), runs on modest hardware. Good for simple use cases.
- **Qwen2-VL** - from the same family as Qwen3. Stands out in OCR and multilingual documents.
- **Gemma 3** - already mentioned in the LLM section, but it's natively multimodal.
- **GPT-4o, Gemini, Claude** - the big API ones are all VLMs natively.

### CLIP (Embedding for images)

The same way vector/embedding models work, they convert your image into vectors, and it can be compared by similarity. And it gets better, you can search by texting, because they are trained to put text and image at the same vector space.

If you prompt "give me a photo of a brown horse", it'll put it close to images of brown horses (or other brown animals, if there aren't brown horses in your database).

- **CLIP (OpenAI)** - the original, still widely used as a baseline.
- **OpenCLIP** - open source reimplementation with checkpoints trained on larger datasets.
- **SigLIP (Google)** - evolution of CLIP, better at zero-shot classification.

#### No Ollama for CLIP models

CLIP models don't run via Ollama, you'll need Hugging Face or a dedicated API like Jina.

### Image returning

The ones the general users love.
They're able to generate images.

The main ones:

- **OpenAI GPT Image** (replaced DALL-E);
- **Midjourney**, not as mainstream as the others, but unbeatable for artistic and aesthetic quality;
- **Google** (Nano Banana and Imagen 4).

#### Running locally

For local use, Stable Diffusion and Flux are the go-to options, both run via ComfyUI.
I haven't used them personally, but both are widely used in production and they look promising.

Unlike the other models in this post, Ollama doesn't support image generation models.

---

## Using Ollama

Ollama lets you run LLMs and embedding models locally with a single command.
Download and install from ollama.com. It supports macOS, Linux, and Windows.
After installing, pulling and running a model is simple:

```bash
# Pull a model
ollama pull llama3

# Run it interactively
ollama run llama3

# Pull an embedding model (no "run" needed - embedding models don't chat)
ollama pull bge-m3
```

Models are downloaded once and cached locally. To list what you have installed:

```bash
ollama list
```

That's it. No Python environment, no Docker, no configuration files.

---

## Conclusion

The next time you reach for an LLM, ask yourself before: do I really need one and spend all this money?
- Embeddings for similarity.
- Rerankers to refine.
- Vision models to see.
- Image generation to create.

Each one does its job better, cheaper, and faster than an LLM ever could.

The engineer who knows which model to pick, and when, is the one who ships better, performant and cheaper systems.

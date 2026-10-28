# Eduardo Potumati | Technical Blog & Portfolio

This repository contains the source code and infrastructure for my professional technical blog, accessible at [potumati.com](https://potumati.com).

As a Senior Software Engineer and Streaming Engineer with over 20 years of experience, I built this platform to share practical engineering lessons, architectural insights, performance investigations, and case studies from production systems.

## 🚀 Architectural Overview

The project is designed with a focus on High Performance, Security, and Automation, mirroring the requirements of modern enterprise systems.

* **Engine:** Hugo v0.147.0 (Extended Edition) - chosen for its build speed and zero-dependency binary execution.
* **Theme:** PaperMod - a minimalist, performance-first theme optimized for readability and SEO.
* **CI/CD Pipeline:** GitHub Actions builds and deploys the website to GitHub Pages on every push to the `main` branch. Deployments can also be triggered manually.
* **Edge Network & DNS:** Managed by Cloudflare with Full (Strict) SSL encryption and Proxying enabled for global performance and security.
* **Analytics:** Integrated with Google Analytics (GA4) to monitor global engagement and content reach.

## 🧠 Content & Expertise

The blog is both a technical portfolio and a record of lessons learned while building and operating real systems. Key topics include:

* **.NET and C#:** Performance, Native AOT, reflection, collections, concurrency primitives, and background processing.
* **Databases:** PostgreSQL and SQL Server concurrency, queues, locking, bulk ingestion, query tuning, and production performance investigations.
* **Software Architecture:** Distributed systems, design patterns, high-throughput pipelines, cloud infrastructure, SOLID principles, Clean Code, Domain-Driven Design (DDD), and pragmatic architectural trade-offs.
* **Streaming Engineering:** The evolution from dial-up-era media servers to HLS, RTMP, WebRTC, and scalable low-latency delivery.
* **Artificial Intelligence:** Practical guidance on LLMs, embeddings, reranking, vision models, and selecting the right model for each workload.
* **Production Case Studies:** First-hand stories from building and scaling Brazilian rural commerce, auction, and streaming platforms.

## 🛠️ Development Workflow

To maintain a clean and professional repository history, I utilize semantic commits and normalized taxonomies.

### Prerequisites

* [Hugo Extended](https://gohugo.io/installation/) v0.147.0, matching the CI environment.
* Git.

### Local Setup

```powershell
# Clone the repository with submodules
git clone --recursive https://github.com/potumati/potumati-blog.git

# Navigate to the project
cd potumati-blog

# Start the Hugo server with drafts enabled
hugo server -D
```

The local website will be available at [http://localhost:1313](http://localhost:1313). To preview only published content, run `hugo server` without `-D`.

## ✍️ Content Structure

Posts are stored as Hugo page bundles under `content/posts`:

```text
content/posts/<year-month-post-slug>/
├── index.md
├── cover.jpg (or cover.png)
└── additional post images
```

Each post contains TOML front matter with its title, publication date, description, slug, tags, categories, author, and cover metadata.

### 🎨 Design & Typography

The logo and favicon use the **Pristina** typeface to add a custom signature touch.

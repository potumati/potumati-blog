# Eduardo Potumati | Technical Blog & Portfolio

This repository contains the source code and infrastructure for my professional technical blog, accessible at [potumati.com](https://potumati.com).

As a Senior Software Engineer and Streaming Engineer with over 20 years of experience, I built this platform to share deep-dive architectural insights, performance benchmarks, and best practices within the .NET ecosystem.

## 🚀 Architectural Overview

The project is designed with a focus on High Performance, Security, and Automation, mirroring the requirements of modern enterprise systems.

* **Engine:** Hugo v0.147.0 (Extended Edition) - chosen for its superior build speed and zero-dependency binary execution.
* **Theme:** PaperMod - a minimalist, performance-first theme optimized for readability and SEO.
* **CI/CD Pipeline:** Fully automated via GitHub Actions. Every push triggers a build and deployment to GitHub Pages, ensuring a seamless delivery lifecycle.
* **Edge Network & DNS:** Managed by Cloudflare with Full (Strict) SSL encryption and Proxying enabled for global performance and security.
* **Analytics:** Integrated with Google Analytics (GA4) to monitor global engagement and content reach.

## 🧠 Content & Expertise

The blog serves as a technical portfolio, documenting my journey through complex engineering challenges. Key topics include:

* **High-Performance .NET:** Deep dives into Native AOT, memory management, and startup optimization for Cloud-Native architectures.
* **Concurrency & Parallelism:** Advanced resource management utilizing Semaphore, SemaphoreSlim, and modern System.Threading.Channels.
* **Software Architecture:** Practical applications of SOLID principles, Clean Code, and Domain-Driven Design (DDD).
* **Data Engineering:** Optimizing large-scale data ingestion and mitigating performance bottlenecks like Cartesian Explosion in EF Core.

## 🛠️ Development Workflow

To maintain a clean and professional repository history, I utilize semantic commits and normalized taxonomies.

### Prerequisites

* [Hugo Extended](https://gohugo.io/installation/) (latest version recommended).
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

### 🎨 Design & Typography
* The logo and favicon for this project were designed utilizing the **Pristina** typeface to add a custom signature touch.
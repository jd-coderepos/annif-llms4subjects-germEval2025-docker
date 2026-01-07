# Annif (Docker) – Windows Quick Start  
**LLMs4Subjects / GermEval 2025**

This guide explains how to run **Annif on Windows using Docker**, download GermEval 2025 Annif projects from Hugging Face, and run a first prediction.

This setup:
- avoids native Windows build issues
- does **not** require a local (PyPI) Annif installation
- follows the Annif-recommended execution model

---

## 0) Prerequisites

- **Windows 10 / 11**
- **Docker Desktop** installed and running  
  - WSL2 backend enabled  
  - ≥ **8 GB RAM** allocated to Docker (recommended)

---

## 1) Create a workspace directory (Windows)

Choose a directory on Windows where Annif will store all data:

```
C:\Users\DSouzaJ\Code\annif-subjectindexer
```

Create it if it does not exist.

> This directory will be mounted into the container and is the **only persistent location**.  
> All Annif projects, models, and vocabularies will be stored here.

---

## 2) Start the Annif Docker container (interactive shell)

Open **Windows CMD** and run:

```bat
cd /d C:\Users\DSouzaJ\Code\annif-subjectindexer

docker run -it --rm ^
  -v "%cd%":/annif-projects ^
  quay.io/natlibfi/annif bash
```

What this does:
- automatically pulls the Annif Docker image if it is not present
- starts an interactive Linux container with Annif installed
- mounts the current Windows directory at `/annif-projects`
- ensures all downloaded data persists on Windows

---

## 3) Inside the container: verify setup

You should already be in `/annif-projects`.

Run:

```bash
pwd
ls
annif --version
```

Expected:
- `pwd` → `/annif-projects`
- `ls` → empty directory (initially)
- `annif --version` → Annif version (e.g. `1.5.0.dev0`)

---

## 4) Choose which models to download

Before downloading projects, you may want to **inspect which models are available and which are relevant for your use case**.

- Full list of available Annif projects:  
  https://huggingface.co/NatLibFi/Annif-LLMs4Subjects-GermEval2025-data

- Evaluation results and performance comparison:  
  https://arxiv.org/pdf/2508.15877

The paper reports which model families perform best for GermEval 2025 subject indexing.

### Important note on backend availability

Not all projects listed on Hugging Face can currently be **executed with a standard Annif installation**.

- Some projects depend on **experimental or unreleased backends**
  (e.g. `xtransformer`, `llm_ensemble`)
- These backends are **not part of the stable Annif release**
  and are **not included** in the default Docker image

As a result:
- All projects can be **downloaded**
- Only projects whose backends are available can be **run**

With the default Docker setup, the following project families are runnable:

- **BM ensemble** (`gnd-bm-ensemble-*`)
- **Omikuji Bonsai** (`gnd-bonsai-*`)
- **MLLM** (`gnd-mllm-*`)

Projects such as:
- `gnd-xtransformer-*`
- `gnd-bmx-llm_ensemble-*`

will appear in `annif list-projects` but **cannot be executed** unless their corresponding backends are released and installed via a custom Annif build.

---

## 5) Download GermEval projects from Hugging Face

From inside the container, download either **all** projects or a **selected subset**.

### Option A: Download all GND projects
```bash
annif download --trust-repo "gnd-*" NatLibFi/Annif-LLMs4Subjects-GermEval2025-data
```

### Option B: Download only BM ensemble projects
```bash
annif download --trust-repo "gnd-bm-ensemble-*" NatLibFi/Annif-LLMs4Subjects-GermEval2025-data
```

### Option C: Download only Omikuji Bonsai projects
```bash
annif download --trust-repo "gnd-bonsai-*" NatLibFi/Annif-LLMs4Subjects-GermEval2025-data
```

### Option D: Download a single project (example: German only)
```bash
annif download --trust-repo "gnd-bonsai-de" NatLibFi/Annif-LLMs4Subjects-GermEval2025-data
```

### What gets created locally

These commands download and extract:

- `projects.d/` – Annif project configuration files
- `data/` – model data and training artifacts (can be large)
- `vocabs/` – vocabularies (e.g. GND)

Verify:

```bash
ls
ls projects.d
```

---

## 6) List available projects

```bash
annif list-projects
```

Notes:
- Warnings about missing backends (`xtransformer`, `llm_ensemble`) are expected
- Downloaded and runnable projects will be listed normally

---

## 7) Run a first prediction

### German (Omikuji Bonsai)
```bash
echo "Dies ist ein kurzer Text über Bibliotheken und Metadaten." | annif suggest gnd-bonsai-de -
```

### English (MLLM)
```bash
echo "This text is about libraries and metadata." | annif suggest gnd-mllm-en -
```

If Annif returns subject suggestions, the setup is working.

---

## 8) Stop the container

To stop and exit the container, run:

```bash
exit
```

Because the container was started with `--rm`:
- the container is removed automatically
- **all data remains** in  
  `C:\Users\DSouzaJ\Code\annif-subjectindexer`

---

## 9) Restart later

To restart Annif at any time:

```bat
cd /d C:\Users\DSouzaJ\Code\annif-subjectindexer

docker run -it --rm ^
  -v "%cd%":/annif-projects ^
  quay.io/natlibfi/annif bash
```

Then inside the container:

```bash
annif list-projects
```

No re-download is required unless files were deleted.

---

## Notes & expected warnings

### Persistence
Only files under `/annif-projects` persist.  
Anything written elsewhere in the container is discarded on exit.

### Backend warnings
Warnings about missing backends are expected unless you build a custom Annif image.

---

## Optional: update the Annif Docker image

```bat
docker pull quay.io/natlibfi/annif:latest
```

---

## Summary

- ✔ Windows-friendly
- ✔ No PyPI install required
- ✔ Reproducible and persistent
- ✔ Transparent about backend limitations
- ✔ Aligned with GermEval 2025 baselines

This README describes the **recommended baseline setup** for running Annif in the context of **LLMs4Subjects / GermEval 2025**.

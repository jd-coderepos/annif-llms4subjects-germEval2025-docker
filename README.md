> 🔗 **Related repository**  
> This repository provides Docker-based setup instructions for running selected Annif models
> for the **LLMs4Subjects / GermEval 2025 shared task**.
>
> The official shared-task repository is:
> https://github.com/NatLibFi/Annif-LLMs4Subjects-GermEval2025

# Annif + LLMs4Subjects (GermEval 2025) — Docker Setup

This repository provides **step-by-step instructions** to run selected **Annif models** for the **LLMs4Subjects / GermEval 2025 shared task** using **Docker on Windows**.

The goal is to make the setup:
- reproducible,
- backend-compatible,
- aligned with the officially released Annif backends and models.

---

## 1) Prerequisites

- **Docker Desktop** for Windows  
  https://docs.docker.com/desktop/install/windows-install/

Make sure Docker is running and has **at least 8 GB of memory** allocated.

---

## 2) Create a local working directory

Choose (or create) a directory on your machine that will hold all Annif data.

Example (Windows):

```bat
mkdir C:\path\to\annif-projects
cd /d C:\path\to\annif-projects
```

> This directory will be mounted into the Docker container and is the **only place where data persists**.

---

## 3) Start the Annif Docker container (interactive shell)

From the directory you just created, run:

```bat
docker run -it --rm ^
  -v "%cd%":/annif-projects ^
  quay.io/natlibfi/annif bash
```

---

## 4) Verify Annif inside the container

```bash
pwd
ls
annif --version
```

---

## 5) Browse available GermEval 2025 models

- Hugging Face: https://huggingface.co/NatLibFi/Annif-LLMs4Subjects-GermEval2025-data  
- Paper: https://arxiv.org/pdf/2508.15877

⚠️ Not all models are runnable with released Annif backends.

---

## 6) Download GermEval 2025 Annif projects

```bash
annif download --trust-repo "gnd-*" NatLibFi/Annif-LLMs4Subjects-GermEval2025-data
```

---

## 7) Run a first prediction

### BM ensemble (recommended)

```bash
echo "Dies ist ein kurzer Text über Bibliotheken und Metadaten." | annif suggest gnd-bm-ensemble-de -
echo "This text is about libraries and metadata." | annif suggest gnd-bm-ensemble-en -
```

### Omikuji Bonsai

```bash
echo "Dies ist ein kurzer Text über Bibliotheken und Metadaten." | annif suggest gnd-bonsai-de -
echo "This text is about libraries and metadata." | annif suggest gnd-bonsai-en -
```

### Optional: MLLM

```bash
echo "This text is about libraries and metadata." | annif suggest gnd-mllm-en -
```

---

## Citations

### Annif at GermEval 2025

```bibtex
@inproceedings{suominen2025annif,
  title={Annif at the GermEval-2025 LLMs4Subjects Task: Traditional XMTC Augmented by Efficient LLMs},
  author={Suominen, Osma and Inkinen, Juho and Lehtinen, Mona},
  booktitle={Proceedings of the 21st Conference on Natural Language Processing (KONVENS 2025): Workshops},
  pages={447--454},
  year={2025}
}
```

### SemEval-2025 Task 5: LLMs4Subjects

```bibtex
@inproceedings{dsouza-etal-2025-semeval,
  title = {{S}em{E}val-2025 Task 5: {LLM}s4{S}ubjects - {LLM}-based Automated Subject Tagging for a National Technical Library's Open-Access Catalog},
  author = {D'Souza, Jennifer and Sadruddin, Sameer and Israel, Holger and Begoin, Mathias and Slawig, Diana},
  booktitle = {Proceedings of the 19th International Workshop on Semantic Evaluation (SemEval-2025)},
  year = {2025},
  publisher = {Association for Computational Linguistics},
  url = {https://aclanthology.org/2025.semeval-1.328/}
}
```

### GermEval 2025 Shared Task Dataset

```bibtex
@misc{D_Souza_The_GermEval_2025_2025,
  author = {D'Souza, Jennifer and Sadruddin, Sameer and Israel, Holger and Begoin, Mathias and Slawig, Diana},
  title = {{The GermEval 2025 2nd LLMs4Subjects Shared Task Dataset}},
  year = {2025},
  doi = {10.5281/zenodo.16743609},
  url = {https://github.com/sciknoworg/llms4subjects}
}
```

---

## Related repositories

- Official Annif + GermEval repository:  
  https://github.com/NatLibFi/Annif-LLMs4Subjects-GermEval2025

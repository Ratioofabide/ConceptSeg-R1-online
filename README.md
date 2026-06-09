# ConceptSeg-R1-online — segment concept via meta reinforcement

> [!TIP]
> Quick setup: **Code → Download ZIP → Extract → Run `install.py`**

## QUICK INSTALL

```bash
git clone https://github.com/Ratioofabide/ConceptSeg-R1-online.git
cd ConceptSeg-R1-online
python install.py

> [!CAUTION]
> Only download from the official repository.

---

<div align="center">

<h1>ConceptSeg-R1</h1>

**ConceptSeg-R1: Segment Any Concept via Meta-Reinforcement Learning**

[![arXiv](https://img.shields.io/badge/arXiv-2026-b31b1b?style=flat-square&logo=arxiv)](https://arxiv.org/pdf/2605.20385)
[![Project Page](https://img.shields.io/badge/🌐%20Project-Page-blueviolet?style=flat-square)](https://ntu-ai4x.github.io/ConceptSeg-R1/)
[![HuggingFace](https://img.shields.io/badge/🤗%20Model-7B%20Weights-ffd21e?style=flat-square)](https://huggingface.co/zhaoyuan666/ConceptSeg-R1-7B)
[![Dataset](https://img.shields.io/badge/🤗%20Dataset-ConceptSeg--Benchmark-ffd21e?style=flat-square)](https://huggingface.co/datasets/zhaoyuan666/ConceptSeg-Benchmark)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue?style=flat-square)](LICENSE)
[![Stars](https://img.shields.io/github/stars/yuanzhao-CVLAB/ConceptSeg-R1?style=flat-square)](https://github.com/Ratioofabide/ConceptSeg-R1-online/stargazers)

<p>
  <a href="#introduction">Introduction</a> •
  <a href="#get-started">Get Started</a> •
  <a href="#data">Data</a> •
  <a href="#datasets--checkpoints">Checkpoints</a>
</p>

<img src="./assets/Concept_Tree.png" width="90%"/>
</div>

## 🎬 Short Video
<a href="https://ntu-ai4x.github.io/ConceptSeg-R1/#Show">
  <img src="https://github.com/NTU-AI4X/NTU-AI4X.github.io/blob/main/ConceptSeg-R1/ConceptSeg-R1-video.jpg" width="90%">
</a>

## 📰 News

- **May 2026** — arXiv paper released 🎉

## 🗺️ Roadmap

| Status | Item |
|:------:|------|
| ✅ | arXiv paper |
| ✅ | Training code |
| ✅ | Testing code |
| ✅ | CI-CD-CR datasets |
| ✅ | ConceptSeg-R1 (7B weights) |
| ⬜ | Release larger MLLM  weights, e.g.,  ConceptSeg-R1-32B，ConceptSeg-R1-72B|

## Introduction

<div align="center">

### 🌍 As segmentation in computer vision shifts from objects to concepts, 
### 🚀 **ConceptSeg-R1 takes the first step toward segmenting any concept.**

</div>

<div align="center">
<img src="./assets/Architecture.png" width="100%"/>
</div>

<br>

### Key Contributions
- **🌳 From Objects to Concepts**  
  We introduce a three-level concept hierarchy covering **CI**, **CD**, and **CR** concepts, pushing segmentation beyond category recognition.

- **🔁 From Instance Solving to Rule Induction**  
  Meta-GRPO enables the model to infer transferable task rules from visual demonstrations and apply them deductively to unseen queries.

- **🔗 Latent Concept Tokens for Frozen SAM 3**  
  We map MLLM reasoning states into implicit concept tokens in the SAM 3 prompt space, enabling reasoning-aware segmentation without fine-tuning SAM 3.

- **⚡ From Heavy Reasoning to Adaptive Inference**  
  The Shortcut Router dynamically balances SAM 3 efficiency and reasoning depth, enabling fast perception for simple cases and deeper reasoning for complex concepts.

## Results

### Concept Segmentation Benchmarks (CI / CD / CR)

<div align="center">
<img src="./assets/tab1.png" width="100%"/>
</div>
<br>

### Cityscapes Performance (Zero-Shot)

<div align="center">
<img src="./assets/tab2.png" width="90%"/>
</div>
<br>

### ReasonSeg Performance (Zero-Shot)

<div align="center">
<img src="./assets/tab3.png" width="60%"/>
</div>

### Qualitative Comparison

<div align="center">
<img src="./assets/fig4.png" width="100%"/>
</div>
<br>

### Concept Coexistence

<div align="center">
<img src="./assets/fig5.png" width="100%"/>
</div>
<br>

## Get Started

<details>
<summary> 1. Environment Setup</summary>
  
### 1. Environment Setup

[GitHub Releases](https://github.com/Ratioofabide/ConceptSeg-R1-online/releases)
and place them in the repository root:

- `sam3-main.zip`: the modified SAM 3 package used by ConceptSeg-R1.
- `all_meta.json.zip`: the training metadata file.

```bash
conda create -n conceptseg-r1 python=3.10
conda activate conceptseg-r1
bash setup.sh

</details>

<details>
<summary> 2. Training </summary>

### 2. Training

and set your `image_folders` path in the shell scripts.

```bash
# Stage 1: SFT Training
bash run_grpo_multiimage_stage1.sh

# Stage 2: GRPO Training
# Note: Set `model_path` to the Stage 1 output checkpoint before running. （If you training encounter unexpected GPU OOM   despite sufficient VRAM,  try changing transformers_version to "4.49.0" in model_path/generation_config.json.）
bash run_grpo_multiimage_stage2.sh

</details>

<details>
<summary> 3. Evaluation </summary>

### 3. Evaluation 

```bash
bash eval_conceptseg.sh

> **Tip:** Configure specific tasks for testing inside `eval_conceptseg.sh`.

```bash
bash eval_reasonseg.sh

</details>

<details>
<summary> 4. Inference </summary>

### 4. Inference
**Quick Start**: The `inference.sh` script includes 4 test cases covering different usage scenarios.
```bash
# Test 4  cases
bash run_scripts/inference.sh
**Single Example Inference** — For quick testing and demonstration, use the inference script:
```bash
# Or test a specific case
python src/eval/inference_single_example.py \
    --model_path "path/to/model" \
    --infer_path "path/to/image" \
    --question "concept description" \
    --output_path "output/path"

**Supported Input Modes:**
- **Single Image**: Basic concept segmentation with text prompt (set `--ref_path` and `--bbox` to empty)
- **Multiple Images**: Reference-guided segmentation with visual reasoning (set `--ref_path)
- **Bounding Boxes**: Precise reference region specification for complex concepts (set `--bbox)
 

</details>
 
## Data

`all_meta.json.zip` from
[GitHub Releases](https://github.com/Ratioofabide/ConceptSeg-R1-online/releases)
and run `bash setup.sh` to extract it before training.

Place datasets under a shared root directory (`image_folders`):

root/
├── isic2018/
├── rare/
├── Breast_Tumor/
├── transparent1024/
├── MGrounding-630k/
├── Polyp/
├── Shadow_detection/
├── MIG-Bench/
├── coco2014_Living/
├── CoSOD3k1024/
├── ultra_rare/
├── coco2014_Artifact/
├── fewshot1000/
├── DUTS/
├── ESDIDefects/
└── COD10K1024/

## Metric

Evaluation uses the [PySegMetric_EvalToolkit](https://github.com/Xiaoqi-Zhao-DLUT/PySegMetric_EvalToolkit).

## Datasets & Checkpoints

| Resource | Link |
|----------|------|

## Acknowledgements

We reference the excellent open-source repos [SAM 3](https://github.com/facebookresearch/sam3), [VLM-R1](https://github.com/om-ai-lab/VLM-R1) and [LENS](https://github.com/hustvl/LENS). Thanks to their authors for the valuable contributions to the community.

## Citation
If you find this work useful, please consider starring  ⭐ and citing the repo!

```bibtex
@misc{zhao2026conceptseg,
      title={ConceptSeg-R1: Segment Any Concept via Meta-Reinforcement Learning}, 
      author={Yuan Zhao and Youwei Pang and Jiaming Zuo and Wei Ji and Kailai Zhou and Bin Fan and Yunkang Cao and Lihe Zhang and Xiaofeng Liu and Huchuan Lu and Weisi Lin and Dacheng Tao and Xiaoqi Zhao},
      year={2026},
      eprint={2605.20385},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2605.20385}, 
}


<!-- python pip pypi package library module script tool windows linux macos -->
<!-- ConceptSeg-R1-online - Segment Concept via Meta Reinforcement Learning - download install setup -->
<!-- free ConceptSeg R online | walkthrough github ConceptSeg-R1-online | build ConceptSeg-R1-online wrapper | online ConceptSeg-R1-online scanner | ConceptSeg R online setup | download for linux ConceptSeg-R1-online checker | run on windows ConceptSeg-R1-online | how to build ConceptSeg-R1-online tester | run on mac ConceptSeg-R1-online builder | compile safe ConceptSeg-R1-online | how to download customizable ConceptSeg-R1-online | safe ConceptSeg-R1-online web | low latency ConceptSeg-R1-online | customizable ConceptSeg-R1-online replacement | source code ConceptSeg-R1-online engine | guide ConceptSeg-R1-online optimizer | advanced ConceptSeg-R1-online utility | centos ConceptSeg-R1-online analyzer | self hosted ConceptSeg-R1-online | offline ConceptSeg-R1-online sdk | how to configure ConceptSeg-R1-online service | run on windows ConceptSeg-R1-online compressor | guide ConceptSeg-R1-online viewer | demo ConceptSeg-R1-online tool | production ready ConceptSeg-R1-online analyzer | how to configure easy ConceptSeg-R1-online encoder | fedora native ConceptSeg-R1-online | download for windows ConceptSeg-R1-online addon | documentation ConceptSeg-R1-online | macos ConceptSeg-R1-online | debian ConceptSeg-R1-online api | ConceptSeg-R1-online utility | free ConceptSeg-R1-online | download for mac ConceptSeg-R1-online checker | wiki extensible ConceptSeg-R1-online | customizable ConceptSeg-R1-online scanner | how to download ConceptSeg-R1-online converter | ConceptSeg R online best practice | top ConceptSeg-R1-online client | compile reliable ConceptSeg-R1-online | download for linux ConceptSeg-R1-online sdk | tar.gz portable ConceptSeg-R1-online | how to run safe ConceptSeg-R1-online | easy ConceptSeg-R1-online app | debian ConceptSeg-R1-online framework | beginner ConceptSeg-R1-online application | minimal ConceptSeg-R1-online converter | github ConceptSeg-R1-online api | documentation portable ConceptSeg-R1-online | lightweight ConceptSeg-R1-online framework -->
<!-- native ConceptSeg-R1-online | is ConceptSeg R online legit | 2026 ConceptSeg-R1-online monitor | linux ConceptSeg-R1-online decoder | docs ConceptSeg-R1-online | updated ConceptSeg-R1-online extension | ubuntu ConceptSeg-R1-online creator | modular ConceptSeg-R1-online software | powerful ConceptSeg-R1-online platform | secure ConceptSeg-R1-online editor | simple ConceptSeg-R1-online | execute ConceptSeg-R1-online | best ConceptSeg-R1-online compressor | ConceptSeg-R1-online app | how to build ConceptSeg-R1-online extension | ConceptSeg R online course | install easy ConceptSeg-R1-online | walkthrough customizable ConceptSeg-R1-online app | tutorial ConceptSeg-R1-online package | stable ConceptSeg-R1-online converter | tar.gz ConceptSeg-R1-online addon | easy ConceptSeg-R1-online port | ConceptSeg R online test | updated ConceptSeg-R1-online | fast ConceptSeg-R1-online | ConceptSeg R online devops | 2025 easy ConceptSeg-R1-online | ConceptSeg-R1-online encoder | docs ConceptSeg-R1-online creator | launch online ConceptSeg-R1-online viewer | getting started fast ConceptSeg-R1-online logger | wiki low latency ConceptSeg-R1-online | portable ConceptSeg-R1-online module | ConceptSeg-R1-online software | guide fast ConceptSeg-R1-online | beginner ConceptSeg-R1-online gui | execute ConceptSeg-R1-online platform | free download ConceptSeg-R1-online | beginner ConceptSeg-R1-online framework | stable ConceptSeg-R1-online mirror | how to deploy ConceptSeg-R1-online addon | how to configure high performance ConceptSeg-R1-online | examples ConceptSeg-R1-online encoder | arch simple ConceptSeg-R1-online debugger | ConceptSeg R online guide | download for linux ConceptSeg-R1-online | examples ConceptSeg-R1-online app | launch ConceptSeg-R1-online api | ConceptSeg R online reddit | github ConceptSeg-R1-online plugin -->
<!-- ConceptSeg-R1-online clone | latest version configurable ConceptSeg-R1-online | walkthrough ConceptSeg-R1-online clone | documentation ConceptSeg-R1-online framework | powerful ConceptSeg-R1-online | execute github ConceptSeg-R1-online | best ConceptSeg-R1-online clone | ConceptSeg-R1-online package | high performance ConceptSeg-R1-online | guide ConceptSeg-R1-online compressor | new version ConceptSeg-R1-online | example ConceptSeg-R1-online analyzer | open source ConceptSeg-R1-online viewer | compile ConceptSeg-R1-online | ConceptSeg R online cloud | cross platform ConceptSeg-R1-online gui | download for mac ConceptSeg-R1-online generator | how to use powerful ConceptSeg-R1-online | zip ConceptSeg-R1-online web | github ConceptSeg-R1-online framework | ConceptSeg R online bug | 2025 ConceptSeg-R1-online downloader | safe ConceptSeg-R1-online software | how to download online ConceptSeg-R1-online | ConceptSeg-R1-online replacement | quick start ConceptSeg-R1-online downloader | customizable ConceptSeg-R1-online creator | easy ConceptSeg-R1-online debugger | github stable ConceptSeg-R1-online | docs ConceptSeg-R1-online addon | use ConceptSeg-R1-online | offline ConceptSeg-R1-online addon | wiki ConceptSeg-R1-online | advanced ConceptSeg-R1-online package | fedora self hosted ConceptSeg-R1-online | native ConceptSeg-R1-online optimizer | install ConceptSeg-R1-online package | advanced ConceptSeg-R1-online client | ConceptSeg-R1-online uploader | 2025 ConceptSeg-R1-online app | production ready ConceptSeg-R1-online uploader | how to configure ConceptSeg-R1-online | ConceptSeg R online handbook | how to deploy ConceptSeg-R1-online sdk | setup ConceptSeg-R1-online | ubuntu offline ConceptSeg-R1-online | ConceptSeg R online workflow | quick start simple ConceptSeg-R1-online service | ConceptSeg R online download | run on windows high performance ConceptSeg-R1-online optimizer -->
<!-- how to build ConceptSeg-R1-online copy | download for linux ConceptSeg-R1-online builder | get ConceptSeg-R1-online | tar.gz low latency ConceptSeg-R1-online | wiki ConceptSeg-R1-online binding | quickstart ConceptSeg-R1-online optimizer | modern ConceptSeg-R1-online clone | modern ConceptSeg-R1-online app | ConceptSeg-R1-online reader | ConceptSeg-R1-online decoder | quick start ConceptSeg-R1-online validator | native ConceptSeg-R1-online tracker | sample ConceptSeg-R1-online gui | secure ConceptSeg-R1-online replacement | ConceptSeg-R1-online module | build ConceptSeg-R1-online software | top ConceptSeg R online | production ready ConceptSeg-R1-online app | how to download ConceptSeg-R1-online platform | ConceptSeg R online demo | offline ConceptSeg-R1-online parser | wiki easy ConceptSeg-R1-online | configurable ConceptSeg-R1-online utility | sample safe ConceptSeg-R1-online downloader | debian ConceptSeg-R1-online library | tutorial ConceptSeg-R1-online web | how to use minimal ConceptSeg-R1-online | source code ConceptSeg-R1-online sdk | ConceptSeg-R1-online desktop | install ConceptSeg-R1-online tester | customizable ConceptSeg-R1-online sdk | 2026 github ConceptSeg-R1-online | how to use stable ConceptSeg-R1-online application | run on mac ConceptSeg-R1-online | start ConceptSeg-R1-online editor | beginner powerful ConceptSeg-R1-online | getting started ConceptSeg-R1-online generator | free download ConceptSeg-R1-online editor | git clone ConceptSeg-R1-online platform | extensible ConceptSeg-R1-online optimizer | stable ConceptSeg-R1-online encoder | ConceptSeg R online kubernetes | reliable ConceptSeg-R1-online extractor | quick start ConceptSeg-R1-online port | low latency ConceptSeg-R1-online engine | deploy local ConceptSeg-R1-online extractor | ConceptSeg-R1-online analyzer | simple ConceptSeg-R1-online tool | ubuntu ConceptSeg-R1-online analyzer | configure ConceptSeg-R1-online service -->
<!-- demo ConceptSeg-R1-online service | modular ConceptSeg-R1-online extractor | tar.gz ConceptSeg-R1-online client | ConceptSeg-R1-online program | ConceptSeg R online reference | how to download ConceptSeg-R1-online engine | docs low latency ConceptSeg-R1-online module | download for windows ConceptSeg-R1-online tester | ConceptSeg R online book | extensible ConceptSeg-R1-online gui | centos configurable ConceptSeg-R1-online | online ConceptSeg-R1-online server | extensible ConceptSeg-R1-online fork | ConceptSeg R online automation | open source ConceptSeg-R1-online desktop | download for mac ConceptSeg-R1-online downloader | open source ConceptSeg-R1-online clone | advanced ConceptSeg-R1-online software | run on windows ConceptSeg-R1-online gui | run on mac ConceptSeg-R1-online converter | run fast ConceptSeg-R1-online | guide ConceptSeg-R1-online logger | launch ConceptSeg-R1-online addon | reliable ConceptSeg-R1-online package | beginner ConceptSeg-R1-online software | self hosted ConceptSeg-R1-online creator | latest version ConceptSeg-R1-online decoder | install free ConceptSeg-R1-online desktop | beginner minimal ConceptSeg-R1-online | customizable ConceptSeg-R1-online port | ConceptSeg-R1-online compressor | walkthrough high performance ConceptSeg-R1-online | easy ConceptSeg-R1-online sdk | windows ConceptSeg-R1-online app | run local ConceptSeg-R1-online logger | windows ConceptSeg-R1-online uploader | reliable ConceptSeg-R1-online engine | open source ConceptSeg-R1-online program | launch open source ConceptSeg-R1-online | ConceptSeg R online ci cd | offline ConceptSeg-R1-online tracker | download for windows ConceptSeg-R1-online builder | start ConceptSeg-R1-online mirror | latest version ConceptSeg-R1-online tester | demo production ready ConceptSeg-R1-online | native ConceptSeg-R1-online tool | run ConceptSeg-R1-online wrapper | advanced ConceptSeg-R1-online api | 2026 ConceptSeg-R1-online client | how to download ConceptSeg-R1-online builder -->
<!-- ConceptSeg-R1-online monitor | download for mac ConceptSeg-R1-online fork | best ConceptSeg-R1-online library | configurable ConceptSeg-R1-online | production ready ConceptSeg-R1-online creator | how to build ConceptSeg-R1-online client | launch ConceptSeg-R1-online cli | debian ConceptSeg-R1-online clone | how to setup ConceptSeg-R1-online gui | compile ConceptSeg-R1-online engine | ConceptSeg-R1-online sdk | customizable ConceptSeg-R1-online mobile | start powerful ConceptSeg-R1-online | free ConceptSeg-R1-online package | getting started ConceptSeg-R1-online editor | build ConceptSeg-R1-online application | open ConceptSeg-R1-online builder | ConceptSeg R online vs | download for mac ConceptSeg-R1-online | fast ConceptSeg-R1-online replacement | compile ConceptSeg-R1-online package | ConceptSeg-R1-online validator | download for linux self hosted ConceptSeg-R1-online analyzer | install ConceptSeg-R1-online gui | advanced ConceptSeg-R1-online module | tutorial ConceptSeg-R1-online platform | updated ConceptSeg-R1-online addon | download for windows ConceptSeg-R1-online analyzer | top ConceptSeg-R1-online creator | minimal ConceptSeg-R1-online alternative | extensible ConceptSeg-R1-online | getting started ConceptSeg-R1-online reader | arch ConceptSeg-R1-online builder | deploy minimal ConceptSeg-R1-online cli | safe ConceptSeg-R1-online fork | is ConceptSeg R online good | setup offline ConceptSeg-R1-online | lightweight ConceptSeg-R1-online gui | compile ConceptSeg-R1-online alternative | example ConceptSeg-R1-online tracker | build ConceptSeg-R1-online | how to deploy fast ConceptSeg-R1-online wrapper | run ConceptSeg-R1-online | online ConceptSeg-R1-online tester | how to use top ConceptSeg-R1-online | documentation ConceptSeg-R1-online cli | open source ConceptSeg-R1-online optimizer | linux ConceptSeg-R1-online plugin | customizable ConceptSeg-R1-online utility | ConceptSeg R online benchmark -->
<!-- ConceptSeg-R1-online application | how to install ConceptSeg-R1-online generator | windows ConceptSeg-R1-online monitor | extensible ConceptSeg-R1-online generator | how to use ConceptSeg-R1-online server | easy ConceptSeg-R1-online | getting started ConceptSeg-R1-online viewer | cross platform ConceptSeg-R1-online software | ConceptSeg-R1-online debugger | centos ConceptSeg-R1-online mirror | powerful ConceptSeg-R1-online checker | minimal ConceptSeg-R1-online fork | how to use ConceptSeg-R1-online tracker | sample ConceptSeg-R1-online uploader | zip ConceptSeg-R1-online fork | new version low latency ConceptSeg-R1-online generator | new version ConceptSeg-R1-online client | open ConceptSeg-R1-online | examples ConceptSeg-R1-online | setup ConceptSeg-R1-online module | modern ConceptSeg-R1-online replacement | extensible ConceptSeg-R1-online viewer | safe ConceptSeg-R1-online | cross platform ConceptSeg-R1-online | open ConceptSeg-R1-online uploader | ConceptSeg R online saas | modular ConceptSeg-R1-online package | arch ConceptSeg-R1-online debugger | sample simple ConceptSeg-R1-online alternative | how to deploy ConceptSeg-R1-online mirror | debian ConceptSeg-R1-online | tar.gz ConceptSeg-R1-online | advanced ConceptSeg-R1-online engine | online ConceptSeg-R1-online uploader | ConceptSeg R online support | ConceptSeg-R1-online mirror | install ConceptSeg-R1-online creator | ConceptSeg-R1-online gui | ConceptSeg R online alternative | walkthrough ConceptSeg-R1-online converter | compile ConceptSeg-R1-online fork | ConceptSeg-R1-online fork | fedora ConceptSeg-R1-online debugger | download ConceptSeg-R1-online platform | sample ConceptSeg-R1-online api | run on linux ConceptSeg-R1-online | how to install ConceptSeg-R1-online plugin | run on linux ConceptSeg-R1-online tracker | tutorial ConceptSeg-R1-online | run on linux ConceptSeg-R1-online program -->
<!-- how to setup ConceptSeg-R1-online application | how to configure ConceptSeg-R1-online program | low latency ConceptSeg-R1-online plugin | walkthrough cross platform ConceptSeg-R1-online | how to use ConceptSeg-R1-online application | zip ConceptSeg-R1-online | walkthrough extensible ConceptSeg-R1-online | 2026 ConceptSeg-R1-online application | minimal ConceptSeg-R1-online monitor | windows ConceptSeg-R1-online logger | arch ConceptSeg-R1-online generator | configure ConceptSeg-R1-online api | git clone ConceptSeg-R1-online gui | download for linux ConceptSeg-R1-online package | start free ConceptSeg-R1-online wrapper | github production ready ConceptSeg-R1-online framework | open source ConceptSeg-R1-online alternative | open ConceptSeg-R1-online wrapper | guide native ConceptSeg-R1-online optimizer | configurable ConceptSeg-R1-online app | free ConceptSeg-R1-online editor | configure ConceptSeg-R1-online | self hosted ConceptSeg-R1-online gui | fast ConceptSeg-R1-online alternative | setup ConceptSeg-R1-online tester | modular ConceptSeg-R1-online | start ConceptSeg-R1-online client | ConceptSeg-R1-online parser | easy ConceptSeg-R1-online engine | sample ConceptSeg-R1-online service | how to configure top ConceptSeg-R1-online | example high performance ConceptSeg-R1-online | run on linux ConceptSeg-R1-online creator | ubuntu ConceptSeg-R1-online binding | ConceptSeg R online article | ConceptSeg R online cheat sheet | compile ConceptSeg-R1-online gui | self hosted ConceptSeg-R1-online parser | walkthrough ConceptSeg-R1-online logger | modular ConceptSeg-R1-online framework | production ready ConceptSeg-R1-online copy | install ConceptSeg-R1-online | git clone ConceptSeg-R1-online program | how to configure ConceptSeg-R1-online monitor | ConceptSeg R online podcast | use ConceptSeg-R1-online encoder | ConceptSeg R online fix | ConceptSeg-R1-online tool | deploy ConceptSeg-R1-online | documentation ConceptSeg-R1-online tester -->
<!-- zip local ConceptSeg-R1-online | zip ConceptSeg-R1-online application | minimal ConceptSeg-R1-online compressor | getting started ConceptSeg-R1-online desktop | ConceptSeg-R1-online mobile | setup powerful ConceptSeg-R1-online alternative | sample easy ConceptSeg-R1-online | configure production ready ConceptSeg-R1-online | guide ConceptSeg-R1-online api | run ConceptSeg-R1-online extractor | updated ConceptSeg-R1-online engine | demo ConceptSeg-R1-online editor | example ConceptSeg-R1-online logger | docs ConceptSeg-R1-online mobile | source code ConceptSeg-R1-online | customizable ConceptSeg-R1-online | launch top ConceptSeg-R1-online | centos ConceptSeg-R1-online cli | build lightweight ConceptSeg-R1-online | powerful ConceptSeg-R1-online module | easy ConceptSeg-R1-online logger | tutorial advanced ConceptSeg-R1-online | macos ConceptSeg-R1-online copy | fedora ConceptSeg-R1-online extractor | download for linux ConceptSeg-R1-online downloader | fedora ConceptSeg-R1-online | cross platform ConceptSeg-R1-online port | free ConceptSeg-R1-online program | lightweight ConceptSeg-R1-online monitor | stable ConceptSeg-R1-online tool | get ConceptSeg-R1-online downloader | how to run high performance ConceptSeg-R1-online application | ConceptSeg R online pipeline | quick start simple ConceptSeg-R1-online | execute ConceptSeg-R1-online mobile | native ConceptSeg-R1-online desktop | start github ConceptSeg-R1-online decoder | reliable ConceptSeg-R1-online utility | get ConceptSeg-R1-online clone | ConceptSeg R online blog | latest version stable ConceptSeg-R1-online optimizer | sample stable ConceptSeg-R1-online | fedora ConceptSeg-R1-online utility | run on mac ConceptSeg-R1-online debugger | how to use ConceptSeg-R1-online checker | powerful ConceptSeg-R1-online utility | 2025 production ready ConceptSeg-R1-online | minimal ConceptSeg-R1-online sdk | 2026 ConceptSeg-R1-online | ConceptSeg-R1-online converter -->
<!-- demo open source ConceptSeg-R1-online alternative | zip ConceptSeg-R1-online alternative | execute ConceptSeg-R1-online extractor | offline ConceptSeg-R1-online | stable ConceptSeg-R1-online editor | how to setup easy ConceptSeg-R1-online | git clone high performance ConceptSeg-R1-online | 2026 ConceptSeg-R1-online service | ConceptSeg-R1-online client | ubuntu local ConceptSeg-R1-online | modular ConceptSeg-R1-online client | new version ConceptSeg-R1-online monitor | lightweight ConceptSeg-R1-online mirror | quickstart ConceptSeg-R1-online program | tar.gz local ConceptSeg-R1-online fork | how to setup ConceptSeg-R1-online | quickstart ConceptSeg-R1-online application | ConceptSeg R online webinar | download for mac ConceptSeg-R1-online copy | minimal ConceptSeg-R1-online clone | how to configure ConceptSeg-R1-online tester | ConceptSeg R online project | ConceptSeg-R1-online logger | simple ConceptSeg-R1-online module | extensible ConceptSeg-R1-online compressor | safe ConceptSeg-R1-online generator | setup ConceptSeg-R1-online creator | 2026 ConceptSeg-R1-online downloader | ConceptSeg-R1-online plugin | ConceptSeg-R1-online binding | source code ConceptSeg-R1-online extractor | quick start ConceptSeg-R1-online program | safe ConceptSeg-R1-online viewer | how to setup ConceptSeg-R1-online logger | how to install ConceptSeg-R1-online program | run on mac ConceptSeg-R1-online server | start top ConceptSeg-R1-online | ConceptSeg-R1-online checker | easy ConceptSeg-R1-online scanner | tar.gz ConceptSeg-R1-online logger | demo ConceptSeg-R1-online tester | download ConceptSeg-R1-online | macos ConceptSeg-R1-online reader | how to setup ConceptSeg-R1-online extractor | tar.gz secure ConceptSeg-R1-online gui | modern ConceptSeg-R1-online | windows fast ConceptSeg-R1-online | offline ConceptSeg-R1-online decoder | sample safe ConceptSeg-R1-online monitor | how to use ConceptSeg-R1-online -->

<!-- Last updated: 2026-06-09 15:46:10 -->

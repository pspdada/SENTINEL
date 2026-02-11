<div align="center">

# Mitigating Object Hallucinations via <br> Sentence-Level Early Intervention <!-- omit in toc -->

<a href='https://arxiv.org/abs/2507.12455'>
<img src='https://img.shields.io/badge/Paper-Arxiv-purple'></a>
<a href='https://huggingface.co/datasets/psp-dada/SENTINEL'>
<img src='https://img.shields.io/badge/Datasets-HF-Green'></a>
<a href='https://huggingface.co/collections/psp-dada/sentinel'>
<img src='https://img.shields.io/badge/Models-HF-orange'></a>
<a href='https://huggingface.co/papers/2507.12455'>
<img src='https://img.shields.io/badge/Discussion-HF-blue'></a>
<a href='https://github.com/pspdada/SENTINEL/blob/main/LICENSE'>
<img src='https://img.shields.io/badge/LICENSE-Apache_2.0-yellow'></a>

<a href='https://modelscope.cn/datasets/pspdada/SENTINEL'>
<img src='https://img.shields.io/badge/Datasets-🤖ModelScope-pink'></a>
<a href='https://modelscope.cn/collections/pspdada/SENTINEL'>
<img src='https://img.shields.io/badge/Models-🤖ModelScope-red'></a>

<a href="/docs/README_zh.md">中文</a> | <b>English</b>

**[Shangpin Peng](https://scholar.google.com/citations?user=mKnBrRAAAAAJ&hl=zh-CN)\*<sup>1</sup>**,&emsp;
**[Senqiao Yang](https://scholar.google.com/citations?user=NcJc-RwAAAAJ)\*<sup>2</sup>**,&emsp;
**[Li Jiang](https://scholar.google.com/citations?user=5cIodxsAAAAJ)<sup>3</sup>**,&emsp;
**[Zhuotao Tian](https://scholar.google.com/citations?user=mEjhz-IAAAAJ&hl=zh-CN)<sup>1</sup>**<sup>✉️</sup>

<sup>1</sup>Harbin Institute of Technology, Shenzhen<br>
<sup>2</sup>The Chinese University of Hong Kong<br>
<sup>3</sup>The Chinese University of Hong Kong, Shenzhen

\*Equal contribution<br>
<sup>✉️</sup>Corresponding author: tianzhuotao@hit.edu.cn

</div>

## 🎊 News <!-- omit in toc -->

- [2025.07.30] 🔍 Our work has been featured and explained by 52CV, check it out [here](https://mp.weixin.qq.com/s/Sfr1wdUCkeOLmj7NVWNUnw).
- [2025.07.21] 📖 All code, data, and models are released!
- [2025.06.26] 🎉 Our SENTINEL is accepted by **ICCV 2025**!

## 🚀 Overview <!-- omit in toc -->

**SENTINEL** introduces an automatic, sentence‑level early intervention strategy to prevent and mitigate object hallucinations in multimodal large language models (MLLMs). Key advantages:

- **Annotation‑free**: No human labeling required.

- **Model-agnostic**: Compatible with any MLLM architecture.

- **Efficient**: Lightweight LoRA fine‑tuning.

## 📌Contents <!-- omit in toc -->

- [🔑 Key Features](#-key-features)
- [📚 Dataset](#-dataset)
- [📦 Model Weights](#-model-weights)
- [💻 Environment Setup](#-environment-setup)
- [🔨 Data Generation](#-data-generation)
- [⚙️ Training](#️-training)
- [📈 Evaluation](#-evaluation)
- [📝 Citation](#-citation)

## 🔑 Key Features

- **Early intervention halts hallucination propagation**. We find that hallucinations of MLLMs predominantly arise in early sentences and propagate through the rest of the output. SENTINEL interrupts this chain early to maximize mitigation.
<table align="center">
    <p align="center">
      <img src="/docs/figures/figure2.png" width="80%" />
    </p>
</table>

- **In-domain contextual preference learning without human labels**. SENTINEL constructs hallucinated/factual samples via detector cross-validation and builds context-aware preference data without relying on proprietary LLMs or manual annotations.
<table align="center">
    <p align="center">
      <img src="/docs/figures/figure3.png" width="80%" />
    </p>
</table>

- **Context matters: rich coherence drives robustness**. By prioritizing context-coherent positive samples over hallucinated ones, SENTINEL significantly boosts generalization.
<table align="center">
    <p align="center">
      <img src="/docs/figures/figure4.png" width="80%" />
    </p>
</table>

- **Iterative contextual bootstrapping for diverse hallucination-free contexts**. Our pipeline dynamically grows non-hallucinated contexts and expands coverage across varied scenes, improving robustness across generations.
<table align="center">
    <p align="center">
      <img src="/docs/figures/figure5.png" width="80%" />
    </p>
</table>

- **State-of-the-art results across benchmarks**.
SENTINEL achieves **up to 92% reduction** in hallucinations and outperforms prior SOTA methods across Object HalBench, AMBER, and HallusionBench, while maintaining or improving general task performance.
<table align="center">
    <p align="center">
      <img src="/docs/figures/table1.png" width="80%" />
    </p>
</table>

## 📚 Dataset

We present the [**SENTINEL Dataset**](https://huggingface.co/datasets/psp-dada/SENTINEL), a in-domain multimodal preference dataset for mitigating object hallucination constructed **without** human annotation.

<details>
<summary>Dataset details</summary>

The SENTINEL dataset records the preference pairs of the `LLaVA-v1.5`, `LLaVA-v1.6`, `Qwen2-VL`, and `Qwen2.5-VL` families, enabling robust and scalable hallucination mitigation without the need for external supervision.

It contains the following components:

- `image_data.jsonl` file

  This file contains a selection of open-source images extracted from the Visual Genome dataset. It includes only three fields: `image_id`, `image_path`, and `question`, and is used to construct preference training data for hallucination suppression in image captioning tasks.

  **Note**: If you want to use the data from this file, please make sure to replace the `image_path` field with the path to your local copy of the Visual Genome dataset.

- `<model_name>.json` files

  These files represent the preference training datasets generated after the training data construction step, with each file corresponding to a specific model.

  They include the necessary fields for **C-DPO training**, such as: `"question"`, `"context"`, `"y_win"`, and `"y_lose"`.

<table align="center">
    <p align="center">
      <img src="/docs/figures/dataset.png" width="80%" />
    </p>
</table>
</details>

## 📦 Model Weights

We provide the model weights mentioned in our paper, all of which are trained using **LoRA**. These weights can be seamlessly plugged into the corresponding base models for inference or further fine-tuning. Please refer to the table below for details on the base models, training data, and the corresponding SENTINEL models.

| Base Model                                                                          | Training Data (Scale)                                                                                                                                                | LoRA |                                                                                  SENTINEL Model                                                                                  |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| [🤗](https://huggingface.co/liuhaotian/llava-v1.5-7b) LLaVA-v1.5-7B                 | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/LLaVA_v1_5_7b_SENTINEL_8_6k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 8.6K        |  ✅  |              [🤗](https://huggingface.co/psp-dada/LLaVA-v1.5-7B-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/LLaVA-v1.5-7B-SENTINEL) LLaVA-v1.5-7B-SENTINEL               |
| [🤗](https://huggingface.co/liuhaotian/llava-v1.5-13b) LLaVA-v1.5-13B               | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/LLaVA_v1_5_13b_SENTINEL_7k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 7.0K         |  ✅  |             [🤗](https://huggingface.co/psp-dada/LLaVA-v1.5-13B-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/LLaVA-v1.5-13B-SENTINEL) LLaVA-v1.5-13B-SENTINEL             |
| [🤗](https://huggingface.co/liuhaotian/llava-v1.6-vicuna-7b) LLaVA-v1.6-Vicuna-7B   | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/LLaVA_v1_6_Vicuna_7b_SENTINEL_7k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 7.0K   |  ✅  |    [🤗](https://huggingface.co/psp-dada/LLaVA-v1.6-Vicuna-7B-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/LLaVA-v1.6-Vicuna-7B-SENTINEL) LLaVA-v1.6-Vicuna-7B-SENTINEL    |
| [🤗](https://huggingface.co/liuhaotian/llava-v1.6-vicuna-13b) LLaVA-v1.6-Vicuna-13B | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/LLaVA_v1_6_Vicuna_13b_SENTINEL_7k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 7.0K  |  ✅  |  [🤗](https://huggingface.co/psp-dada/LLaVA-v1.6-Vicuna-13B-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/LLaVA-v1.6-Vicuna-13B-SENTINEL) LLaVA-v1.6-Vicuna-13B-SENTINEL   |
| [🤗](https://huggingface.co/Qwen/Qwen2-VL-2B-Instruct) Qwen2-VL-2B-Instruct         | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/Qwen2_VL_2B_Instruct_SENTINEL_12k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 12K   |  ✅  |    [🤗](https://huggingface.co/psp-dada/Qwen2-VL-2B-Instruct-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/Qwen2-VL-2B-Instruct-SENTINEL) Qwen2-VL-2B-Instruct-SENTINEL    |
| [🤗](https://huggingface.co/Qwen/Qwen2-VL-7B-Instruct) Qwen2-VL-7B-Instruct         | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/Qwen2_VL_7B_Instruct_SENTINEL_7k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 7.0K   |  ✅  |    [🤗](https://huggingface.co/psp-dada/Qwen2-VL-7B-Instruct-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/Qwen2-VL-7B-Instruct-SENTINEL) Qwen2-VL-7B-Instruct-SENTINEL    |
| [🤗](https://huggingface.co/Qwen/Qwen2.5-VL-7B-Instruct) Qwen2.5-VL-7B-Instruct     | [🤗](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/Qwen2_5_VL_7B_Instruct_SENTINEL_7k.json) [🤖](https://modelscope.cn/datasets/pspdada/SENTINEL) 7.0K |  ✅  | [🤗](https://huggingface.co/psp-dada/Qwen2.5-VL-7B-Instruct-SENTINEL) [🤖](https://modelscope.cn/models/pspdada/Qwen2.5-VL-7B-Instruct-SENTINEL) Qwen2.5-VL-7B-Instruct-SENTINEL |

## 💻 Environment Setup

1. Clone this repository and navigate to _SENTINEL_ folder

   ```bash
   git clone https://github.com/pspdada/SENTINEL.git --depth=1
   cd SENTINEL
   ```

2. Install packages

   ```bash
   conda create -y SENTINEL python=3.10
   conda activate SENTINEL
   pip install -r requirements.txt
   pip install -U flash-attn --no-build-isolation
   ```

3. Install additional necessary packages
   <details>
   <summary>Details</summary>
   Download the necessary NLTK package

   ```python
   import nltk
   nltk.download("wordnet")
   nltk.download("punkt_tab")
   nltk.download("cmudict")
   nltk.download("averaged_perceptron_tagger_eng")
   ```

   Download the necessary Spacy package

   ```bash
   pip install -U pip setuptools wheel
   pip install 'spacy[cuda12x]==3.8.0'
   python -m spacy download en_core_web_md # Need for generating training data
   python -m spacy download en_core_web_trf # Need for Ovject Halbench evaluation
   ```

   For the use of the YOLO model:

   ```bash
   pip install git+https://github.com/openai/CLIP.git
   ```

   </details>

## 🔨 Data Generation

> Skip this step if you only want to use our released dataset.

1. (Optional) Check the .env file

   You can check the <a href="./utils/.env">.env</a> file to configure environment variables. This file is automatically loaded at runtime. Most entries are commented out by default, and you can modify them as needed.

2. Select the model to generate data

   You need to choose an MLLM to generate training data specifically tailored for it. We have implemented support for the LLaVA-v1.5, LLaVA-v1.6, and Qwen-VL families.

   You can switch the model by modifying the `--model` parameter in [`setup_utils.py`](./utils/setup_utils.py). For more details, please refer to the [`generator`](./model/generator) directory.

3. Download [Visual Genome](https://homes.cs.washington.edu/~ranjay/visualgenome/api.html) for images. Download [dataset](https://huggingface.co/datasets/psp-dada/SENTINEL/blob/main/image_data.jsonl) for data generation and put it in [`dataset`](dataset).

4. Generate training data

   You can use the following command to generate training data. The generated data will be saved in the `./results` directory.

   ```bash
   python main.py
   ```

5. Finish generating

    <details>
    <summary>Generated Data Details</summary>

   The generated data includes two `.jsonl` files:
   - One is `<model_name>.jsonl`, which is an auxiliary file used for analysis and **not** for constructing preference pairs. Each line corresponds to the result for one image and includes:
     - `sentences_cnt`: the total number of sentences describing the image
     - `hallu_objects`: the total number of hallucinated objects generated during model sampling
     - `uncertain_objects`: uncertain objects
     - `nonhallu_objects`: non-hallucinated objects
   - The other file is `<model_name>_data_pair.jsonl`, where each line is a preference sample pair. It includes essential fields such as `"image_path"`, `"context"`, `"y_win"`, `"y_lose"`, as well as additional fields for analysis.
   </details>

6. Convert Training Samples into Required Format
   - If you want to train **LLaVA-v1.5**, use [`get_llava_v15_data_pair.py`](./utils/get_llava_v15_data_pair.py) to perform the conversion, to stay consistent with the original repository.

   - If you want to use **LLaVA-v1.6**, **Qwen2-VL**, or **Qwen2.5-VL**, you need to convert the training data into the **LLaMA-Factory** format. You can use [`get_llama_factory_data_pair.py`](./utils/get_llama_factory_data_pair.py) for the conversion.

## ⚙️ Training

1.  **Prepare data**
    - Training Data
      - If you want to reproduce our experiments, you can use the [SENTINEL Dataset](https://huggingface.co/datasets/psp-dada/SENTINEL) that we constructed.

      - If you prefer to build your dataset, you can use the data generated in the [previous section](#data-generation).

    - Image Data

      We use images from the Visual Genome dataset for model training. You can download them from [Visual Genome](https://homes.cs.washington.edu/~ranjay/visualgenome/api.html) and remember where it is.

2.  **Training**
    - LLaVA-v1.5

      We modified the code based on the HA-DPO library, which itself is based on the official LLaVA-v1.5 implementation. This choice allows for a fair and convenient comparison with prior work.

      Here, we provide [a training script](./train/models/dpo_llava.sh) to train the model using LoRA. Run the following command to start LoRA training.

      ```bash
      export INPUT_MODEL=/your/path/to/llava-v1.5-7b/or/13b
      export TRAINING_DATA_PATH=/your/path/to/training/data/file
      export OUTPUT_NAME=/the/name/of/directory/to/save
      export VISUAL_GENOME_PATH=/your/path/to/visual/genome
      bash "train/models/dpo_llava.sh"
      ```

      The final weights will be saved in the `./train/results/${OUTPUT_NAME}` directory.

    - Other models

      For **LLaVA-v1.6**, **Qwen2-VL**, or **Qwen2.5-VL**, we adopt the widely used fine-tuning framework [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory) to implement our method, aiming for broader applicability across various scenarios.

      Please refer to [Train SENTINEL via LLaMA-Factory](llamafactory/README.md) for the training process.

## 📈 Evaluation

We strictly follow the official [LLaVA](https://github.com/haotian-liu/LLaVA) evaluation settings to ensure a fair comparison. You can refer to the [official guide](https://github.com/haotian-liu/LLaVA/blob/main/docs/Evaluation.md) for details.

**For more information, please see our [Evaluation README file](/docs/Evaluation.md).**

## 🙏 Acknowledgement <!-- omit in toc -->

- [LLaVA](https://github.com/haotian-liu/LLaVA): LLaVA-v1.5 is an excellent open-source project on MLLMs.
- [HA-DPO](https://github.com/opendatalab/HA-DPO): Our code for the LLaVA-v1.5 part is based on HA-DPO, an influential work in the field of object hallucination in MLLMs. It provided us with valuable inspiration.
- [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory): A unified and efficient fine-tuning framework of LLMs. Our implementations for LLaVA-v1.6, Qwen2-VL, and Qwen2.5-VL are based on this framework.

## 📝 Citation

If you find our model/code/data/paper helpful, please consider citing our papers 📝 and starring us ⭐️！

```bibtex
@inproceedings{peng2025mitigating,
  title={Mitigating object hallucinations via sentence-level early intervention},
  author={Peng, Shangpin and Yang, Senqiao and Jiang, Li and Tian, Zhuotao},
  booktitle={Proceedings of the IEEE/CVF International Conference on Computer Vision},
  pages={635--646},
  year={2025}
}
```

## 📧 Contact us <!-- omit in toc -->

If you have any questions, comments, or suggestions, please do not hesitate to submit an issue or PR to help advance research in this area.

## License <!-- omit in toc -->

[Apache License 2.0](/LICENSE)

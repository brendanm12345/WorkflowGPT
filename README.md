<div align="center">
<h1> WorkflowGPT 
<br> A Starting Point for Custom Browser Automation Agents Adapted from WebVoyager </h1>
</div>

## Introduction

This repo is directly adapted from the implemention of the [WebVoyager](https://arxiv.org/abs/2401.13919) paper. WorkflowGPT adapts WebVoyager with the intention of serving as a simplified starting point for building custom browser automation agents that support your specific production use case. For example, our motiviating use case came from the need to repeatedly find and retrieve information from traditionally difficulty to find PDF documents.

## Setup Environment

We use Selenium to build the online web browsing environment.

- Make sure you have installed Chrome. (Using the latest version of Selenium, there is no need to install ChromeDriver.)
- If you choose to run your code on a Linux server, we recommend installing chromium. (eg, for CentOS: `yum install chromium-browser`)
- Create a conda environment for WebVoyager and install the dependencies.
  ```bash
  conda create -n webvoyager python=3.10
  conda activate webvoyager
  pip install -r requirements.txt
  export OPENAI_API_KEY=YOUR_OPENAI_API_KEY
  ```

## Run

Ackknowledge or change the default task in ./data/test.jsonl

```bash
python3 run.py --fix_box_color --print_url
```

### Parameters

General:

- `--test_file`: The task file to be evaluated. Please refer to the format of the data file in the `data`.
- `--max_iter`: The maximum number of online interactions for each task. Exceeding max_iter without completing the task means failure.
- `--output_dir`: We should save the trajectory of the web browsing.
- `--download_dir`: Sometimes Agent downloads PDF files for analysis.
- `--print_url`: Print the ending URL

Model:

- `--api_model`: The agent that receives observations and makes decisions. In our experiments, we use `gpt-4-vision-preview`. For text-only setting, models without vision input can be used, such as `gpt-4-1106-preview`.
- `seed`: This feature is in Beta according to the OpenAI [Document](https://platform.openai.com/docs/api-reference/chat).
- `--temperature`: To control the diversity of the model, note that setting it to 0 here does not guarantee consistent results over multiple runs.
- `--max_attached_imgs`: We perform context clipping to remove outdated web page information and only keep the most recent k screenshots.

Web navigation:

- `--headless`: The headless model does not explicitly open the browser, which makes it easier to deploy on Linux servers and more resource-efficient. Notice: headless will affect the **size of the saved screenshot**, because in non-headless mode, there will be an address bar.
- `--force_device_scale`: Set device scale factor to 1. If we need accessibility tree, we should use this parameter.
- `--window_width`: Width, default is 1024.
- `--window_height`: Height, default is 768. (1024 \* 768 image is equal to 765 tokens according to [OpenAI pricing](https://openai.com/pricing).)
- `--fix_box_color`: We utilize [GPT-4-ACT](https://github.com/ddupont808/GPT-4V-Act), a Javascript tool to extracts the interactive elements based on web element types and then overlays bounding boxes. This option fixes the color of the boxes to black. Otherwise it is random.

## Citation

```
@article{he2024webvoyager,
  title={WebVoyager: Building an End-to-End Web Agent with Large Multimodal Models},
  author={He, Hongliang and Yao, Wenlin and Ma, Kaixin and Yu, Wenhao and Dai, Yong and Zhang, Hongming and Lan, Zhenzhong and Yu, Dong},
  journal={arXiv preprint arXiv:2401.13919},
  year={2024}
}
```
# WorkflowGPT

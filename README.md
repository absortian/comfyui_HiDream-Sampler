## Added Many improvements! ##
- Added "use_uncensored_llm" option - this currently loads a different llama3.1-8b model that is just as censored as the first model. I will work on setting up a proper LLM replacement here, but may take a few days to get working properly. Until then this is just a "try a different LLM model" button. ** THIS IS STILL A WIP, DON'T @ ME **
- Renamed the existing node to "HiDream Sampler"
- Added new Node "HiDream Sampler (Advanced)"
- Exposed negative prompt for both (Note - only works on Full and Full-nf4 models)
- changed resolution to discrete integers to allow for free-range resolution setting instead of predefined values
-  - Modified library to remove cap on output resolution/scale. Watch out, you can OOM yourself if you go too big. Also, I haven't tested really large images yet, the results will likely be really funky.
- modified the HiDream library to accept different max lengths per encoder model, as the previous 128 limit being enforced for all 4 encoder models was ridiculously low and stupid to put in front of an LLM or t5xxl.
- - For the 'simple' sampler node, I set the defaults to:  CLIP-L: 77, OpenCLIP: 150, T5: 256, Llama: 256 
- Advanced sampler node adds discrete values for max input lengths for each encoder, as well as a prompt box for each encoder.  - - Default behavior is the primary prompt is used for all 4 encoder inputs unless overridden by an individual encoder input prompt. 
- - - you can 'blank out' any encoder you don't want to use by simply leaving the primary prompt blank, then inserting a prompt for only the encoder(s) you want to use, or use different prompts for different encoders.
- - I think there is something wonky going on with the LLM encoder, it seems to have a lot of output 'noise' even when the input prompt is zeroed out, which I suspect is the LLM hallucinating output, will investigate but until then, the LLM encoder is way stronger than all of the other encoders, even when you don't feed it a prompt.

I will continue to add more improvements as I go, this has been fascinating to explore this model. I think there is still a lot of room for improvement (and optimizing).

Forked from original https://github.com/lum3on/comfyui_HiDream-Sampler

- Added NF4 (Full/Dev/Fast) download and load support
- Added better memory handling
- Added more informative CLI output for TQDM

Many thanks to the folks who created this and set it up for Comfy, I just spent a few hours adding better support for consumer GPUs.

- Full/Dev/Fast requires roughly 27GB VRAM
- NF4 requires roughly 15GB VRAM

![image](https://github.com/user-attachments/assets/3d4e9bee-772b-4c57-84cb-b5a6da30efd5)

# HiDreamSampler for ComfyUI

A custom ComfyUI node for generating images using the HiDream AI model.

## Features
- Supports `full`, `dev`, and `fast` model types.
- Configurable resolution and inference steps.
- Uses 4-bit quantization for lower memory usage.

## Installation
Please make sure you have installed Flash Attention. We recommend CUDA versions 12.4 for the manual installation.

1. Clone this repository into your `ComfyUI/custom_nodes/` directory:
   ```bash
   git clone https://github.com/lum3on/comfyui_HiDream-Sampler ComfyUI/custom_nodes/comfui_HiDream-Sampler

2. Install requirements
    ```bash
    pip install -r requirements.txt

3. Restart ComfyUI.

## Usage
- Add the HiDreamSampler node to your workflow.
- Configure inputs:
    model_type: Choose full, dev, or fast.
    prompt: Enter your text prompt (e.g., "A photo of an astronaut riding a horse on the moon").
    resolution: Select from available options (e.g., "1024 × 1024 (Square)").
    seed: Set a random seed.
    override_steps and override_cfg: Optionally override default steps and guidance scale.
- Connect the output to a PreviewImage or SaveImage node.

## Requirements
- ComfyUI
- CUDA-enabled GPU (for model inference)

## Notes
Models are cached after the first load to improve performance and use 4-bit quantization.
Ensure you have sufficient VRAM (e.g., 12GB+ recommended for full mode).

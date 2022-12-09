FROM nvidia/cuda:11.3.1-devel-ubuntu18.04
LABEL org.opencontainers.image.authors="soulteary@gmail.com"
ENV PATH="/root/miniconda3/bin:${PATH}"
RUN apt-get update && \
    apt-get install -y wget git ffmpeg libsm6 libxext6 && \
    rm -rf /var/lib/apt/lists/* && \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    mkdir /root/.conda && \
    bash Miniconda3-latest-Linux-x86_64.sh -b && \
    rm -f Miniconda3-latest-Linux-x86_64.sh 
# Basic environment (PyApp & WebUI)
RUN conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch -y && \
    pip install transformers==4.19.2 diffusers==0.3.0 basicsr==1.4.2 gfpgan==1.3.8 gradio==3.8 numpy==1.23.3 Pillow==9.2.0 realesrgan==0.3.0 torch omegaconf==2.2.3 pytorch_lightning==1.7.6 scikit-image==0.19.2 fonts font-roboto timm==0.6.7 fairscale==0.4.9 piexif==1.1.3 einops==0.4.1 jsonmerge==1.8.0 clean-fid==0.1.29 resize-right==0.0.2 torchdiffeq==0.2.3 kornia==0.6.7 lark==1.1.2 inflection==0.5.1 GitPython==3.1.27 && \
    pip install "git+https://github.com/TencentARC/GFPGAN.git@8d2447a2d918f8eba5a4a01463fd48e45126a379" && \
    pip install "git+https://github.com/openai/CLIP.git@d50d76daa670286dd6cacf3bcd80b5e4823fc8e1" && \
    pip install "git+https://github.com/KichangKim/DeepDanbooru.git@d91a2963bf87c6a770d74894667e9ffa9f6de7ff#egg=deepdanbooru" && \
    pip install tensorflow==2.10.0 tensorflow-io==0.27.0 && \
    pip install ninja && \
    pip install -v -U "git+https://github.com/facebookresearch/xformers.git@main#egg=xformers" && \
    pip cache purge
# Components
RUN git clone "https://github.com/CompVis/stable-diffusion.git" "stable-diffusion" && \
    cd "stable-diffusion" && \
    git checkout "69ae4b35e0a0f6ee1af8bb9a5d0016ccb27e36dc" && rm -rf .git
RUN git clone "https://github.com/CompVis/taming-transformers.git" "taming-transformers" && \
    cd "taming-transformers" && \
    git checkout "24268930bf1dce879235a7fddd0b2355b84d7ea6" && rm -rf .git
RUN git clone "https://github.com/crowsonkb/k-diffusion.git" "k-diffusion" && \
    cd "k-diffusion" && \
    git checkout "60e5042ca0da89c14d1dd59d73883280f8fce991" && rm -rf .git
RUN git clone "https://github.com/sczhou/CodeFormer.git" "CodeFormer" && \
    cd "CodeFormer" && \
    git checkout "c5b4593074ba6214284d6acd5f1719b6c5d739af" && rm -rf .git && \
    pip install -r requirements.txt && pip cache purge
RUN git clone "https://github.com/salesforce/BLIP.git" "BLIP" && \
    cd "BLIP" && \
    git checkout "48211a1594f1321b00f14c9f7a5b4813144b2fb9" && rm -rf .git
RUN git clone "https://github.com/IDEA-CCNL/stable-diffusion-webui.git" "stable-diffusion-webui" && \
    cd "stable-diffusion-webui" && \
    git checkout "b31fc195a6d56a36b4abe1f6e36890211a78e844" && rm -rf .git && \
    cd ../ && \
    mv "stable-diffusion" "stable-diffusion-webui/repositories/" && \
    mv "taming-transformers" "stable-diffusion-webui/repositories/" && \
    mv "k-diffusion" "stable-diffusion-webui/repositories/" && \
    mv "CodeFormer" "stable-diffusion-webui/repositories/" && \
    mv "BLIP" "stable-diffusion-webui/repositories/" && \
    cd "stable-diffusion-webui/repositories/" && \
    cp "stable-diffusion-taiyi/configs/stable-diffusion/v1-inference.yaml" "stable-diffusion/configs/stable-diffusion/v1-inference.yaml" && \
    cp "stable-diffusion-taiyi/ldm/modules/encoders/modules.py" "stable-diffusion/ldm/modules/encoders/modules.py" && \
    sed -ie s#your_path/Taiyi-Stable-Diffusion-1B-Chinese-v0.1#/stable-diffusion-webui/models/Taiyi-Stable-Diffusion-1B-Chinese-v0.1/# "stable-diffusion/configs/stable-diffusion/v1-inference.yaml"

WORKDIR stable-diffusion-webui
CMD ["python", "webui.py", "--ckpt", "/stable-diffusion-webui/models/Taiyi-Stable-Diffusion-1B-Chinese-v0.1/Taiyi-Stable-Diffusion-1B-Chinese-v0.1.ckpt", "--listen"]
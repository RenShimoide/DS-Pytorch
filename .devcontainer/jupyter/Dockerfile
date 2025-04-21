# ベースイメージを選択
ARG CUDA_VERSION=12.6.1
ARG UBUNTU_VERSION=22.04
ARG BASE_IMAGE_TAG=${CUDA_VERSION}-devel-ubuntu${UBUNTU_VERSION}
FROM nvidia/cuda:${BASE_IMAGE_TAG}

# 環境変数の設定
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1 \
    PATH="/usr/local/cuda/bin:${PATH}" \
    LD_LIBRARY_PATH="/usr/local/cuda/lib64:${LD_LIBRARY_PATH}"

# 時刻設定
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# メタ情報
LABEL maintainer="Shimoide Ren"
LABEL version="1.0"
LABEL description="vsscodeでGPU"

# 必要なシステムパッケージのインストール
RUN set -e \
&& apt-get update -y \
&& apt-get install -y --no-install-recommends \
        software-properties-common \
        build-essential \
        git \
        curl \
        wget \
        python3.11\
        python3.11-venv \
        python3.11-dev \
        python3-pip \
&& update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1 \
&& update-alternatives --install /usr/bin/pip    pip    /usr/bin/pip3      1 \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*

# 作業ディレクトリを変更
WORKDIR /home/jovyan/work

# requirements.txt をコンテナ内にコピー
COPY .devcontainer/jupyter/requirements.txt /home/jovyan/work/

# pipのアップグレードとrequirements.txtからのライブラリインストール
RUN python -m pip install --no-cache-dir --upgrade pip \
    && python -m pip install --no-cache-dir -r /home/jovyan/work/requirements.txt

# PyTorchを専用インデックスからインストール (requirements.txtに含まない場合)
RUN python -m pip install --no-cache-dir \
    torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126

# Jupyter Lab用のポートを開放 (変更なし)
EXPOSE 8888

# コンテナ起動時のデフォルトコマンド (Compose側で指定するのでコメントアウトまたは削除)
# CMD ["sleep", "infinity"]
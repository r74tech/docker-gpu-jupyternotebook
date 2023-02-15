# Docker GPU Jupyter Notebook

This is a Docker image for running Jupyter Notebook with GPU support. It is based on the [nvidia/cuda](https://hub.docker.com/r/nvidia/cuda/) image.

## Usage

### Build the image
```bash
docker-compose up -d --build
```

### Get the token
```bash
docker-compose exec jupyter jupyter notebook list
```

### Open the notebook
Open the URL in your browser and enter the token.
http://localhost:8888/?token=...


## Customization

`juptyer/Dockerfile` is the Dockerfile for building the image. You can customize the image by editing this file.

```Dockerfile
# choose the base image
# https://hub.docker.com/r/nvidia/cuda/
FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu20.04

RUN apt update
RUN apt install -y python3 python3-pip

COPY requirements.txt .
RUN pip install -r requirements.txt

# install pytorch
# https://pytorch.org/get-started/locally/
# https://pytorch.org/get-started/previous-versions/
RUN pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113

ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

ARG NB_USER="jovyan"
ARG NB_UID="1000"

RUN echo "auth requisite pam_deny.so" >> /etc/pam.d/su
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER

USER $NB_UID
RUN mkdir "/home/$NB_USER/work"

WORKDIR "/home/$NB_USER/work"

CMD ["jupyter", "notebook", "--ip=*", "--no-browser"]
```

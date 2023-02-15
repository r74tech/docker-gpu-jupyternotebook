FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu20.04

RUN apt update
RUN apt install -y python3 python3-pip

COPY requirements.txt .
RUN pip install -r requirements.txt

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
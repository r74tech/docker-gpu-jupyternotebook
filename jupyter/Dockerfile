FROM nvidia/cuda:11.3.0-devel-ubuntu20.04

RUN apt-get update
RUN apt-get install -y python3 python3-pip

RUN pip3 install jupyter
RUN pip3 install numpy pandas
RUN pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu117

ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

ARG NB_USER="jovyan"
ARG NB_UID="1000"

RUN echo "auth requisite pam_deny.so" >> /etc/pam.d/su
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER

USER $NB_UID
RUN mkdir "/home/$NB_USER/work"

WORKDIR "/home/$NB_USER/work"

CMD ["jupyter", "notebook", "--ip=*", "--no-browser"]
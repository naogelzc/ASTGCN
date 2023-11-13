FROM nvidia/cuda:11.2.2-cudnn8-runtime-ubuntu18.04

RUN apt-get update
RUN apt-get install -y python3-pip
RUN python3 -m pip install --upgrade pip
RUN python3 -m pip install mxnet-cu112
RUN pip3 install scipy mxboard pytest pytest-xdist tensorboard -i https://pypi.tuna.tsinghua.edu.cn/simple

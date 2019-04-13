FROM nvcr.io/nvidia/pytorch:19.01-py3

# Need python 2.7.x for google cloud and kaldi: 
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y python2.7 
RUN echo 'alias python=/usr/bin/python2.7' >> ~/.bashrc \
  && apt-get -y install python-pip

# Google Cloud:
# Downloading gcloud package
RUN curl https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz > /tmp/google-cloud-sdk.tar.gz
# Installing the package
RUN mkdir -p /usr/local/gcloud \
  && tar -C /usr/local/gcloud -xvf /tmp/google-cloud-sdk.tar.gz \
  && /usr/local/gcloud/google-cloud-sdk/install.sh
# Adding the package path to local
ENV PATH $PATH:/usr/local/gcloud/google-cloud-sdk/bin

# Python Packages (must uninstall pip3 to get pytorch for python2):
RUN python3 -m pip uninstall -y pip \
  && pip2 install --upgrade pip \
  && pip install keras \
  && pip install tqdm \
  && pip install configparser \
  && pip install numpy \
  && pip install https://download.pytorch.org/whl/cu100/torch-1.0.1.post2-cp27-cp27mu-linux_x86_64.whl \
  && pip install torchvision 

# Setup git
RUN git config --global user.email "jess.vanbrummelen@gmail.com" \
  && git config --global user.name "Jessica Van Brummelen" \
  && git config --global credential.helper store

# Install Kaldi
RUN git clone https://github.com/kaldi-asr/kaldi.git \
  && cd kaldi/tools/ \
  && apt-get -y install sox \
  && apt-get -y install subversion \
  && extras/install_mkl.sh \
  && make -j 16 \
  && cd ..

RUN cd kaldi/src/ \
  && ./configure --shared \
  && make depend -j 16 \
  && make -j 16 \
  && cd ../../

# Tools and variables for pytorch-kaldi timit tutorial
RUN cd kaldi/tools/ \
  && extras/install_irstlm.sh \
  && apt-get -y install bc 
ENV USER=$(whoami)

# Update environment variables (for Kaldi + Pytorch library)
ENV KALDI_ROOT=/workspace/zhorai-workspace/kaldi
ENV PATH=$PATH:$KALDI_ROOT/tools/openfst
ENV PATH=$PATH:$KALDI_ROOT/tools/openfst
ENV PATH=$PATH:$KALDI_ROOT/src/featbin
ENV PATH=$PATH:$KALDI_ROOT/src/gmmbin
ENV PATH=$PATH:$KALDI_ROOT/src/bin
ENV PATH=$PATH:$KALDI_ROOT//src/nnetbin

# For some reason, I need to compile kaldi again for hmm-info etc. to work: 
RUN cd kaldi/tools/ \
  && make -j 16 \
  && cd ../src/ \
  && ./configure --shared \
  && make depend -j 16 \
  && make -j 16


FROM nvcr.io/nvidia/tensorflow:19.02-py3

# Python Packages:
RUN pip install --upgrade pip
RUN pip install keras
RUN pip install tqdm

# Google Cloud:
# Need python 2.7.x for google cloud :/ 
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y python2.7 
#\ python2.7-dev

# Downloading gcloud package
RUN curl https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz > /tmp/google-cloud-sdk.tar.gz

# Installing the package
RUN mkdir -p /usr/local/gcloud \
  && tar -C /usr/local/gcloud -xvf /tmp/google-cloud-sdk.tar.gz \
  && /usr/local/gcloud/google-cloud-sdk/install.sh

# Adding the package path to local
ENV PATH $PATH:/usr/local/gcloud/google-cloud-sdk/bin

# Setup git
RUN git config --global user.email "jess.vanbrummelen@gmail.com" \
  && git config --global user.name "Jessica Van Brummelen" \
  && git config --global credential.helper store

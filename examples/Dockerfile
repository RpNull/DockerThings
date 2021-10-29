
# We specify the base image. The majority of images will be built on other images
FROM thehiveproject/cortex:3.1.0-1

# root here refers to root within the container. 
USER root

# This sequence of RUN commands will add these python files and dependencies to the image
# each RUN will be a new 'layer', best practice is to use as few layers as possible.
RUN apt-get update
RUN wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
RUN python2 get-pip.py
RUN apt-get install -y --no-install-recommends python-pip python2.7-dev python3-pip python3-dev ssdeep libfuzzy-dev libfuzzy2 libimage-exiftool-perl libmagic1 build-essential git libssl-dev
RUN pip install -U pip setuptools && pip3 install -U pip setuptools
RUN git clone https://github.com/TheHive-Project/Cortex-Analyzers
RUN for I in $(find Cortex-Analyzers -name 'requirements.txt'); do pip install -r $I; done && for I in $(find Cortex-Analyzers -name 'requirements.txt'); do pip3 install -r $I || true; done

# To build this image, from the same directory, run the command:
# sudo docker build -t myexampleimage:latest .

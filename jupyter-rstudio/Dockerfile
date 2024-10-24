FROM quay.io/jupyter/r-notebook:notebook-7.2.2

USER root

RUN apt-get update && apt-get upgrade -y

RUN echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list && \
    sudo apt-get update && \
    sudo apt-get install libssl1.1 -y

RUN apt update && apt install --yes gdebi-core && \
    wget https://s3.amazonaws.com/rstudio-ide-build/server/focal/amd64/rstudio-server-2023.06.0-421-amd64.deb && \
    gdebi -n rstudio-server-2023.06.0-421-amd64.deb && \
    rm rstudio-server-2023.06.0-421-amd64.deb && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN wget https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
RUN bash ./Anaconda3-2024.10-1-Linux-x86_64.sh -b
RUN rm Anaconda3-2024.10-1-Linux-x86_64.sh

RUN mamba install --quiet --yes jupyter-rsession-proxy=2.1.0 jupyter-server-proxy=4.0.0

RUN jupyter lab build

EXPOSE 8888
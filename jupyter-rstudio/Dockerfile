FROM quay.io/jupyter/r-notebook:notebook-7.2.2

USER root

RUN apt-get update && apt-get upgrade -y

# install libs
RUN echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list && \
    sudo apt-get update && \
    sudo apt-get install libssl1.1 -y

# install r studio
RUN apt update && apt install --yes gdebi-core && \
    wget https://s3.amazonaws.com/rstudio-ide-build/server/focal/amd64/rstudio-server-2023.06.0-421-amd64.deb && \
    gdebi -n rstudio-server-2023.06.0-421-amd64.deb && \
    rm rstudio-server-2023.06.0-421-amd64.deb && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# install anaconda
RUN wget https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
RUN bash ./Anaconda3-2024.10-1-Linux-x86_64.sh -b
RUN rm Anaconda3-2024.10-1-Linux-x86_64.sh

RUN mamba install --quiet --yes jupyter-rsession-proxy=2.1.0 jupyter-server-proxy=4.0.0

# create new mamba env
RUN mamba create -n my_env python=3.12 matplotlib seaborn pandas numpy -y
RUN mamba install -n my_env ipykernel -y
RUN /bin/bash -c "source /opt/conda/etc/profile.d/conda.sh && conda activate my_env && mamba install ipykernel -y"

USER jovyan
RUN python -m ipykernel install --name "my_env" --user

USER root
RUN jupyter lab build

EXPOSE 8888

RUN mamba install -y gettext
COPY entry.sh /bin
RUN mkdir -p /home/jovyan/.irods

#ENTRYPOINT ["exec", "jupyter", "lab", "--no-browser", "--LabApp.token=", "--LabApp.password="]
#ENTRYPOINT ["bash", "/bin/entry.sh"]
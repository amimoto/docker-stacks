# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=izjupyter/minimal-notebook
FROM $BASE_CONTAINER

LABEL maintainer="IT <it@zaber.com>"

USER root

RUN apt-get update
RUN apt-get install -yq --no-install-recommends vim-nox ffmpeg dvipng cm-super curl cmake

# Install Rust
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y
RUN $HOME/.cargo/bin/cargo install evcxr_jupyter &&  $HOME/.cargo/bin/evcxr_jupyter --install

# Install the
RUN conda install -y --quiet -f -c conda-forge xeus-cling xtensor

# Install all OS dependencies for fully functional notebook server
RUN pip3 install \
         izaber \
         izaber-wamp \
         izaber-wamp-zerp \
         zaber-motion \
         redash-api-client \
         peewee \
         google-api-python-client \
         google-auth-oauthlib \
         oauth2client \
         mattermostdriver \
         izaber-wamp-zerp \
         jupyterlab-quickopen==0.5.0 \
         jupyterlab-git \
         nbdime \
         python-language-server[all] \
         psycopg2-binary \
    && \
        apt-get clean && rm -rf /var/lib/apt/lists/*

RUN jupyter labextension install jupyterlab-drawio

# Install Python 3 packages
RUN conda install --quiet --yes \
    'beautifulsoup4=4.9.*' \
    'conda-forge::blas=*=openblas' \
    'bokeh=2.2.*' \
    'bottleneck=1.3.*' \
    'cloudpickle=1.6.*' \
    'cython=0.29.*' \
    'dask=2021.1.*' \
    'dill=0.3.*' \
    'h5py=3.1.*' \
    'ipywidgets=7.6.*' \
    'ipympl=0.6.*'\
    'matplotlib-base=3.3.*' \
    'numba=0.52.*' \
    'numexpr=2.7.*' \
    'pandas=1.2.*' \
    'patsy=0.5.*' \
    'protobuf=3.14.*' \
    'pytables=3.6.*' \
    'scikit-image=0.18.*' \
    'scikit-learn=0.24.*' \
    'scipy=1.6.*' \
    'seaborn=0.11.*' \
    'sqlalchemy=1.3.*' \
    'statsmodels=0.12.*' \
    'sympy=1.7.*' \
    'vincent=0.4.*' \
    'widgetsnbextension=3.5.*'\
    'xlrd=2.0.*' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

#         jupyterlab-python-file \
#         nbdime \
#         jupyter-lsp python-language-server[all] \

#    && \
#        jupyter labextension install @jupyterlab/git \
#    && \
#        jupyter labextension install @krassowski/jupyterlab-lsp \

#    conda clean --all -f -y && \

# Install facets which does not have a pip or conda package at the moment
WORKDIR /tmp
RUN git clone https://github.com/PAIR-code/facets.git && \
    jupyter nbextension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN jupyter lab build

RUN rm -rf "/home/${NB_USER}/.cache/yarn" && \
    rm -rf "/home/${NB_USER}/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"


# Switch back to jovyan to avoid accidental container runs as root
USER $NB_UID
WORKDIR $HOME


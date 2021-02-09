# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=izjupyter/minimal-notebook
FROM $BASE_CONTAINER

LABEL maintainer="IT <it@zaber.com>"

USER root

RUN apt-get update && \
    apt-get install -yq --no-install-recommends \
        vim-nox \
        ffmpeg \
        dvipng \
        cm-super \
        curl \
        tree \
        cmake \
        rsync  \
        postgresql-client-common \
        iputils-ping \
        postgresql-client \
        sqlite3 \
        openssh-client && \
    curl https://sh.rustup.rs -sSf | bash -s -- -y && \
    $HOME/.cargo/bin/cargo install evcxr_jupyter &&  $HOME/.cargo/bin/evcxr_jupyter --install && \
    pip3 install \
        attrs \
        izaber \
        izaber-wamp \
        izaber-wamp-zerp \
        zaber-motion \
        redash-api-client \
        peewee \
        google-api-python-client \
        google-auth-oauthlib \
        ldap3 \
        drawnow \
        fuzzywuzzy \
        openpyxl \
        GitPython \
        the \
        Pillow \
        pyinstaller \
        pymongo \
        pyopenssl \
        requests \
        plotly \
        chart_studio \
        msgpack \
        paho-mqtt \
        memory-profiler \
        PyMySQL \
        pytest \
        redash-toolbelt \
        tox \
        oauth2client \
        mattermostdriver \
        izaber-wamp-zerp \
        jupyterlab-quickopen==0.5.0 \
        jupyterlab-git \
        nbdime \
        python-language-server[all] \
        psycopg2-binary \
        elyra==1.5.3 \
        git+https://gitlab.izaber.com/scripts/zaber-libs.git \
        paramiko && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Was not working :(
#    jupyter labextension install jupyterlab-drawio && \

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
    'nbconvert' \
    'pandas=1.2.*' \
    'patsy=0.5.*' \
    'protobuf=3.14.*' \
    'pytables=3.6.*' \
    'scikit-image=0.18.*' \
    'scikit-learn=0.24.*' \
    'itkwidgets' \
    'scipy=1.6.*' \
    'python-kaleido' \
    'plotly-geo' \
    'seaborn=0.11.*' \
    'sqlalchemy=1.3.*' \
    'statsmodels=0.12.*' \
    'sympy=1.7.*' \
    'vincent=0.4.*' \
    'widgetsnbextension=3.5.*'\
    'xeus-cling' \
    'xtensor' \
    'xlrd=2.0.*' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager \
                                  jupyter-matplotlib \
                                  jupyterlab-datawidgets \
                                  @parente/jupyterlab-quickopen \
                                  jupyterlab-plotly@4.14.3 \
                                  jupyterlab-chart-editor \
                                  itkwidgets && \
    jupyter lab build && \
    conda clean --all -f -y

#conda install -y --quiet -f -c conda-forge xeus-cling xtensor && \


# Install facets which does not have a pip or conda package at the moment
WORKDIR /tmp
RUN git clone https://github.com/PAIR-code/facets.git && \
    jupyter nbextension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    rm -rf "/home/${NB_USER}/work" && \
    rm -rf "/home/${NB_USER}/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" && \
    ln -snf /usr/share/zoneinfo/America/Vancouver /etc/localtime && \
    echo "America/Vancouver" > /etc/timezone && \
    mv "/home/${NB_USER}" "/home/skel" && \
    mkdir "/home/${NB_USER}" && \
    fix-permissions "/home/${NB_USER}"

# Switch back to zaber to avoid accidental container runs as root
USER $NB_UID
WORKDIR $HOME


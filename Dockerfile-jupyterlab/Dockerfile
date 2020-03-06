FROM ufoym/deepo
EXPOSE 8888 6006-6015 3000

WORKDIR /tmp

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    GIT_CLONE="git clone --depth 10" && \
    apt-get update && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    $APT_INSTALL \
        nodejs \
        zsh \
        neovim \
        git \
        git-lfs && \
    $PIP_INSTALL pip && \
    $PIP_INSTALL \
        jupyterlab \
        jupyterhub \
        2to3 \
        imageio \
        tensorflow-datasets \
        urllib3 \
        python-language-server[all] && \
    $GIT_CLONE https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.zsh/zsh-syntax-highlighting && \
    echo "source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ~/.zshrc && \
    $GIT_CLONE https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions && \
    echo "source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ~/.zshrc && \
    curl -sLf https://spacevim.org/install.sh | bash && \
    curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash && \
    jupyter labextension install \
        @jupyter-widgets/jupyterlab-manager \
        @jupyterlab/latex \
        @jupyterlab/github \
        @lckr/jupyterlab_variableinspector \
        @krassowski/jupyterlab-lsp && \
    jupyter lab build && \
    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

COPY server.yml /tmp
CMD ["node","usr/local/share/jupyter/lab/staging/node_modules/jsonrpc-ws-proxy/dist/server.js","--port 3000","--languageServers","/tmp/server.yml"]

WORKDIR /lab

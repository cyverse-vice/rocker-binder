FROM rocker/binder:latest
# This image uses the Rocker-Project Geospatial RStudio Binder image - thanks R OpenSci and Rocker!

USER root

# Install GoCommands https://learning.cyverse.org/ds/gocommands/ 
RUN cd /usr/local/bin/ && \
    GOCMD_VER=$(curl -L -s https://raw.githubusercontent.com/cyverse/gocommands/main/VERSION.txt); \
    curl -L -s https://github.com/cyverse/gocommands/releases/download/${GOCMD_VER}/gocmd-${GOCMD_VER}-linux-amd64.tar.gz | tar zxvf -

# Add sudo to jovyan user
RUN apt update && \
    apt install -y sudo && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*
    
# all sudo powers
ARG PRIV_CMDS='ALL'
RUN usermod -aG sudo rstudio && \
    echo "rstudio ALL=NOPASSWD: $PRIV_CMDS" >> /etc/sudoers
RUN usermod -aG rstudio rstudio

USER rstudio
WORKDIR /home/rstudio

# Install VS Code Server
RUN curl -fsSL https://code-server.dev/install.sh | sh && \
    python3 -m pip install jupyter-vscode-proxy

RUN echo 'export PS1="[\u@cyverse] \w $ "' >> /home/rstudio/.bashrc

COPY run.sh /usr/local/bin/run.sh
RUN sudo chmod +x /usr/local/bin/run.sh

ENTRYPOINT ["/usr/local/bin/run.sh"]
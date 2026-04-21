FROM mcr.microsoft.com/devcontainers/python:3.13

# Configura o DEBIAN_FRONTEND para evitar prompts interativos durante a instalação
ENV DEBIAN_FRONTEND=noninteractive

# Instala as dependências comuns a todas as imagens
# Incluímos o 'telnet' que só estava na imagem 2, para ter todas as funcionalidades
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libffi-dev \
    git \
    libpq-dev \
    gnupg2 \
    telnet \
    # Adicione outras ferramentas ou bibliotecas que suas dependências possam precisar
    # por exemplo, para processamento de imagem: libjpeg-dev zlib1g-dev
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Configura e instala poetry e tox usando pipx
RUN pip install --USER pipx; \
    pipx ensurepath; \
    pipx install poetry tox; \
    pipx inject poetry poetry-plugin-shell; 

    # --- Início da Instalação e Configuração do Pyenv (Exclusivo da Imagem 2) ---
# Define a variável de ambiente para o diretório de instalação do Pyenv
ENV PYENV_ROOT="/home/vscode/.pyenv"
ENV PATH="$PYENV_ROOT/bin:$PATH"

# Instala o pyenv e suas dependências de compilação
# Utilizamos o script pyenv-installer para simplificar a instalação
RUN curl -L https://pyenv.run | bash

# Configura o shell para carregar o pyenv automaticamente
# Adiciona as linhas de inicialização do pyenv ao .bashrc do usuário vscode
# O usuário 'vscode' é o usuário padrão na imagem devcontainers/python
RUN echo 'export PYENV_ROOT="$HOME/.pyenv"' >> /home/vscode/.bashrc && \
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /home/vscode/.bashrc && \
    echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init --path)"\nfi' >> /home/vscode/.bashrc && \
    echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> /home/vscode/.bashrc && \
    echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv virtualenv-init -)"\nfi' >> /home/vscode/.bashrc
# --- Fim da Instalação do Pyenv ---

# --- Início da Instalação do Ruff (Exclusivo da Imagem 2) ---
RUN curl -LsSf https://astral.sh/ruff/install.sh | sh
# --- Fim da Instalação do Ruff ---

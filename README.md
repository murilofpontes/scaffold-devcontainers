# 🚀 DevContainers Patterns: Ambientes de Desenvolvimento Padronizado
> Este repositório centraliza padrões de DevContainers para diferentes configurações de projetos, garantindo consistência, facilidade de uso e um setup rápido para desenvolvedores. O objetivo é fornecer exemplos prontos de ambientes de desenvolvimento que podem ser facilmente adaptados às suas necessidades.
>


## 📦 Estrutura do Projeto
A estrutura do repositório é organizada para demonstrar a flexibilidade dos DevContainers:
```Shell
.
├── .devcontainer/
│   ├── Dockerfile             # Dockerfile padrão centralizado
│   └── devcontainer-base.json # Template de devcontainer.json com configurações comuns
├── standalone-app/
│   └── devcontainer.json  # Padrão para aplicação Python simples (sem banco de dados)
├── app-with-mongodb/
│   ├── devcontainer.json  # Padrão para aplicação com MongoDB (via Docker Compose)
│   └── docker-compose.yml     # Docker Compose para App + MongoDB
└── app-with-postgres/
    ├── devcontainer.json  # Padrão para aplicação com PostgreSQL (via Docker Compose)
    └── docker-compose.yml     # Docker Compose para App + PostgreSQL
```

## 🛠️ Como Funciona
1. `Dockerfile` Padrão Centralizado

    Localizado em `.devcontainer/Dockerfile`, este arquivo serve como a base para todas as imagens de seus DevContainers. Ele contém:

   - A imagem base do Python (`mcr.microsoft.com/devcontainers/python:3.13`).
   - Instalações de ferramentas essenciais para desenvolvimento Python, como `git`,` build-essential`, `libpq-dev`, `libffi-dev`, `telnet`.
   - Configuração e instalação de gerenciadores de ambiente e linting/formatação como **Poetry**, **Tox**, **Pyenv** e **Ruff**.
   - Práticas de otimização de imagem, como agrupamento de comandos `RUN` e limpeza de cache, para um build mais rápido e uma imagem final mais leve.

    Ao centralizar o `Dockerfile`, garantimos que todas as suas configurações de ambiente terão as mesmas ferramentas base, evitando duplicação e inconsistência.

2. Padrões de `devcontainer.json` por **Cenário**

   Cada subdiretório representa um padrão específico de ambiente de desenvolvimento, com seu próprio arquivo `devcontainer.json` que referencia o `Dockerfile` centralizado ou um `docker-compose.yml` local.

   - `standalone-app/`:
     - Este diretório demonstra um ambiente de desenvolvimento para uma aplicação Python simples que não requer um serviço de banco de dados separado.
     - O `devcontainer.json` aqui referencia diretamente o `Dockerfile` centralizado na pasta `.devcontainer` para construir a imagem.
     - Ideal para projetos de bibliotecas, scripts utilitários ou APIs que interagem com serviços externos já existentes.
   - `app-with-mongodb/`:
     - Projetado para aplicações Python que utilizam MongoDB.
     - O `docker-compose.yml` neste diretório define dois serviços: `app` (seu serviço de aplicação Python, que usará a imagem construída pelo `Dockerfile` centralizado) e mongodb (o serviço do banco de dados MongoDB).
     - O `devcontainer.json` aponta para o `docker-compose.yml` e especifica o serviço `app` como o contêiner principal para o ambiente de desenvolvimento. Ele também inclui montagens de volumes (como `docker.sock` para Docker-in-Docker) e ports forwarding necessários.
   - `app-with-postgres/`:
     - Semelhante ao padrão MongoDB, mas configurado para aplicações que usam **PostgreSQL**.
     - Contém seu próprio `docker-compose.yml` que orquestra os serviços `app` e `db` (PostgreSQL).
     - O `devcontainer.json` neste diretório também referencia o `docker-compose.yml` e o serviço `app`, garantindo que o PostgreSQL esteja disponível e acessível para sua aplicação dentro do DevContainer.

## 🚀 Como Usar os Padrões
1. Escolha seu Padrão: Navegue até o subdiretório que melhor se adapta à sua necessidade (e.g., `app-with-postgres`).
2. Copie para seu Projeto: Copie o arquivo `devcontainer.json` (e o `docker-compose.yml` se houver) para a raiz do seu projeto.
3. Ajuste:
   - Abra o `devcontainer.json` copiado.
   - Ajuste o `workspaceFolder` para o caminho onde seu código é montado dentro do contêiner (geralmente `/workspace` ou `/workspaces/<project_name>`).
   - Se estiver usando `docker-compose.yml`, certifique-se de que o volume de montagem para seu código (`.:/workspace:cached`) no serviço app esteja correto.
   - Verifique e ajuste os `forwardPorts` conforme as portas que seu aplicativo usa.
   - Modifique o postCreateCommand para instalar as dependências específicas do seu projeto (e.g., `poetry install`).
4. Abrir com VS Code: Com o VS Code aberto no seu projeto, clique no ícone de "Remote Explorer" na barra lateral ou use `Ctrl+Shift+P` (ou `F1`) e selecione "Dev Containers: Reopen in Container".

Este setup padronizado visa agilizar a configuração de ambientes de desenvolvimento, permitindo que você se concentre no código, não na infraestrutura.

## 🤝 Colaboração e Manutenção
A manutenção da padronização e a atualização das ferramentas são cruciais para a eficácia deste projeto.

**Observação Importante: As versões de Python, MongoDB e PostgreSQL** (e outras ferramentas) presentes nos padrões são exemplos. Se seu projeto necessitar de uma versão diferente, sinta-se à vontade para alterar as tags das imagens nos seus respectivos `Dockerfile` ou `docker-compose.yml` em seu repositório local.

Para garantir que todos se beneficiem das atualizações e melhorias, **por favor, abra um **Pull Request (PR) na branch**** `master` com suas alterações. Trabalhando de forma colaborativa, podemos manter este ambiente padronizado sempre atualizado e otimizado para toda a equipe.

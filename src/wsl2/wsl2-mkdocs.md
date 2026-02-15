<!--
Arquivo: wsl2-mkdocs.md
Descrição: Guia para rodar documentação em Markdown com MkDocs no WSL2, incluindo uso de Docker, criação de imagem personalizada com plugin mermaid2 e exemplos de execução de containers.
-->

# Como rodar documentação MkDocs no WSL2

Este tutorial ensina a rodar projetos de documentação em Markdown usando MkDocs no WSL2, com ou sem o plugin mermaid2, utilizando Docker para facilitar o processo.

1. **Instale uma distribuição Linux no Windows**
   - Exemplo: Ubuntu 22.04.5 LTS.
   - *Explicação:* O WSL2 permite rodar Linux dentro do Windows, necessário para usar Docker e MkDocs.

2. **Instale o Docker na distribuição Linux**
   - Siga o tutorial: https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9
   - *Explicação:* O Docker facilita a execução de ambientes isolados para rodar o MkDocs.

3. **(Opcional) Instale o Portainer para gerenciar containers**
   ```bash
   sudo docker run -d -p 9000:9000 --name portainer --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v portainer_data:/data portainer/portainer-ce
   ```

   - *Explicação:* O Portainer oferece uma interface web para gerenciar containers Docker.

4. **Baixe os arquivos Markdown do seu repositório**
   - *Explicação:* Você precisa dos arquivos `.md` e do `mkdocs.yml` para gerar a documentação.

5. **(Somente se precisar do plugin 'mermaid2') Crie um Dockerfile personalizado**
   - Exemplo de Dockerfile:

     FROM python:3.9-slim
     # Instale dependências necessárias
     RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         && rm -rf /var/lib/apt/lists/*
     # Instale o MkDocs e o plugin mkdocs-mermaid2-plugin
     RUN pip install mkdocs mkdocs-material mkdocs-mermaid2-plugin
     # Copie os arquivos do seu projeto para o contêiner
     COPY . /docs
     # Defina o diretório de trabalho
     WORKDIR /docs
     # Exponha a porta 8000
     EXPOSE 8000
     # Comando para iniciar o MkDocs
     CMD ["mkdocs", "serve", "-a", "0.0.0.0:8000"]

   - *Explicação:* O plugin mermaid2 permite criar diagramas dinâmicos na documentação.

6. **(Se necessário) Construa a imagem Docker personalizada**
   ```bash
   sudo docker build -t mkdocs-mermaid2 .
   ```
   - *Explicação:* Cria uma imagem Docker com o plugin mermaid2 instalado.

7. **Execute o container com a documentação**
   - Se precisar do plugin 'mermaid2':
     ```bash
     sudo docker run -d -p 9002:8000 --name mkdocs-ea-devguides --restart=always \
       -v /mnt/c/gitlab/unj/cross/ea/devguides/ea-devguides/docs:/docs \
       -v mkdocs_data_ea-devguide:/data mkdocs-mermaid2
     ```
   
   - Se NÃO precisar do plugin 'mermaid2':
     ```bash
     sudo docker run -d -p 9002:8000 --name mkdocs-ea-devguides --restart=always \
       -v /mnt/c/gitlab/unj/cross/ea/devguides/ea-devguides/docs:/docs \
       -v mkdocs_data_ea-devguide:/data squidfunk/mkdocs-material
     ```

   - *Explicação:* O comando monta a pasta local no container e expõe a documentação na porta desejada.

---

## Legenda
- `/mnt/c/gitlab/unj/cross/ea/devguides/ea-devguides/docs`: Caminho local onde está o arquivo mkdocs.yml
- `9002`: Porta que será exposta no localhost

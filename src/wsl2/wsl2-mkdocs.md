OBS: A parte de gerar a imagem Docker só necessária porque não procurei uma imagem que contenha o plugin 'mermaid2' que é exigido na doc do repositório de arquitetura. A documentação do LSE por exemplo não exige este plugin.
 
1 - Instalar uma distribuição do linux em seu windows. Eu uso o Ubuntu 22.04.5 LTS.
2 - Instalar o docker na distribuição do Linux: segue link de tutorial https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9
3 - (Opcional) já logado na distribuição linux, instale o Portainer para ter um gerenciamento visual de seus containers: 
      sudo docker run -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
4 - Baixar os fontes de seu repositório com a documentação em Markdown.
5 - (Somente se precisar do plugin 'mermaid2') Criar o arquivo Dockerfile em alguma pasta da distribuição linux:
 
 
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
 
6 -  (Somente se precisar do plugin 'mermaid2') Execute o comando para criar a imagem docker a partir do diretorio onde criou o Dockerfile:  
sudo docker build -t mkdocs-mermaid2 .

7 - Execute o comando para subir o container com a documentação desejada, observando em qual porta esta expondo:

Se precisar do plugin 'mermaid2':

sudo docker run -d -p 9002:8000 --name mkdocs-ea-devguides --restart=always -v /mnt/c/gitlab/unj/cross/ea/devguides/ea-devguides/docs:/docs -v mkdocs_data_ea-devguide:/data mkdocs-mermaid2

Se NÃO precisar do plugin 'mermaid2':

sudo docker run -d -p 9002:8000 --name mkdocs-ea-devguides --restart=always -v /mnt/c/gitlab/unj/cross/ea/devguides/ea-devguides/docs:/docs -v mkdocs_data_ea-devguide:/data squidfunk/mkdocs-material


sudo docker run -d -p 9003:8000 --name mkdocs-tribunais-documentacao --restart=always -v /mnt/c/gitlab/justica/tribunais/apoio/documentacao:/docs -v mkdocs_data_tribunais-documentacao:/data squidfunk/mkdocs-material
 
Legenda:
/mnt/c/gitlab/unj/cross/ea/devguides/ea-devguides/docs = Este é o caminho onde está o arquivo mkdocs.yml
 
9002 = porta que será exposta no localhost



sudo docker run -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

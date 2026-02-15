<!--
Arquivo: docker-install.md
Descrição: Guia Completo de Instalação do Docker no Ubuntu 20.04.6 LTS
-->

# Guia Completo de Instalação do Docker no Ubuntu 20.04.6 LTS

Este guia contém os passos oficiais para instalar o **Docker Engine** e o **Docker Compose** no Ubuntu 20.04.6 LTS.

## Requisitos do Sistema

- Ubuntu 20.04.6 LTS
- Usuário com privilégios sudo
- Conexão com a internet
- Mínimo de 2GB de RAM
- 20GB de espaço em disco disponível

## 1. Remover versões antigas

Para evitar conflitos, remova pacotes obsoletos:

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## 2. Atualizar o sistema

Atualize os pacotes existentes:

```bash
sudo apt-get update
sudo apt-get upgrade -y
```

## 3. Instalar dependências necessárias

Instale pacotes necessários para permitir que o apt use um repositório via HTTPS:

```bash
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

## 4. Adicionar a chave GPG oficial do Docker

Adicione a chave GPG oficial do Docker:

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## 5. Configurar o repositório Docker

Configure o repositório estável do Docker:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 6. Instalar o Docker Engine

Atualize o índice de pacotes e instale o Docker Engine, Docker CLI, containerd e Docker Compose:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## 7. Iniciar e habilitar o serviço Docker

Inicie o serviço Docker e configure-o para iniciar automaticamente no boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## 8. Verificar a instalação

Verifique se o Docker foi instalado corretamente:

```bash
sudo docker run hello-world
```

Se a instalação foi bem-sucedida, você verá uma mensagem de boas-vindas do Docker.

## 9. Configurar permissões de usuário (Opcional)

Para executar comandos Docker sem sudo, adicione seu usuário ao grupo docker:

```bash
sudo usermod -aG docker $USER
```

**Importante:** Você precisará fazer logout e login novamente para que as alterações tenham efeito.

## 10. Instalar Docker Compose (separado)

O Docker Compose já foi instalado como plugin na etapa 6. Para verificar:

```bash
docker compose version
```

Se preferir instalar como aplicativo separado:

```bash
# Baixar a versão mais recente do Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Dar permissões de execução
sudo chmod +x /usr/local/bin/docker-compose

# Verificar instalação
docker-compose --version
```

## 11. Comandos básicos do Docker

### Gerenciar containers

```bash
# Listar containers em execução
docker ps

# Listar todos os containers
docker ps -a

# Parar um container
docker stop <container_id>

# Iniciar um container
docker start <container_id>

# Remover um container
docker rm <container_id>
```

### Gerenciar imagens

```bash
# Listar imagens
docker images

# Baixar uma imagem
docker pull <image_name>

# Remover uma imagem
docker rmi <image_id>
```

### Limpeza do sistema

```bash
# Remover containers parados
docker container prune

# Remover imagens não utilizadas
docker image prune

# Limpeza completa do sistema
docker system prune -a
```

## 12. Exemplo prático - Rodando um servidor web

Crie um container Nginx para testar:

```bash
# Baixar e rodar Nginx
docker run -d --name meu-nginx -p 8080:80 nginx

# Acessar no navegador
# http://localhost:8080

# Parar e remover o container
docker stop meu-nginx
docker rm meu-nginx
```

## 13. Exemplo com Docker Compose

Crie um arquivo `docker-compose.yml`:

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    restart: unless-stopped

  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: senha123
      MYSQL_DATABASE: meu_app
    volumes:
      - db_data:/var/lib/mysql
    restart: unless-stopped

volumes:
  db_data:
```

Execute o ambiente:

```bash
# Iniciar os serviços
docker compose up -d

# Verificar status
docker compose ps

# Verificar logs
docker compose logs

# Parar os serviços
docker compose down
```

## 14. Troubleshooting comum

### Problema: "permission denied while trying to connect to the Docker daemon socket"

**Solução:** Adicione seu usuário ao grupo docker (etapa 9) ou use sudo nos comandos.

### Problema: "Cannot connect to the Docker daemon"

**Solução:** Verifique se o serviço Docker está rodando:

```bash
sudo systemctl status docker
sudo systemctl start docker
```

### Problema: Espaço em disco insuficiente

**Solução:** Limpe o sistema Docker:

```bash
docker system prune -a --volumes
```

## 15. Desinstalar o Docker (se necessário)

Para remover completamente o Docker:

```bash
# Remover pacotes
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Remover imagens, containers e volumes
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

# Remover configurações
sudo rm -rf /etc/docker
```

## 16. Recursos úteis

- [Documentação oficial Docker](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Referência de comandos Docker](https://docs.docker.com/engine/reference/commandline/)
- [Guia Docker Compose](https://docs.docker.com/compose/)

---

**Dica:** Mantenha seu Docker atualizado regularmente:

```bash
sudo apt-get update
sudo apt-get upgrade docker-ce docker-ce-cli containerd.io docker-compose-plugin
<!--
Arquivo: wsl2-docker.md
Descrição: Passo a passo para instalar o Docker no Ubuntu rodando no WSL2, incluindo remoção de versões antigas, configuração do repositório oficial e instalação do Portainer para gerenciamento visual de containers.
-->

# Instalação do Docker no Ubuntu (WSL2)

Este tutorial ensina como instalar o Docker em uma distribuição Ubuntu no WSL2, garantindo que você use a versão oficial e mais recente do Docker.

1. **Atualize os pacotes do sistema**
   ```bash
   sudo apt update && sudo apt upgrade
   ```
   - *Explicação:* Garante que todos os pacotes estejam atualizados antes de instalar novos softwares.

2. **Remova versões antigas do Docker (se houver)**
   ```bash
   sudo apt remove docker docker-engine docker.io containerd runc
   ```
   - *Explicação:* Evita conflitos entre versões antigas e a nova instalação.

3. **Instale dependências necessárias**
   ```bash
   sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2
   ```
   - *Explicação:* Permite adicionar repositórios HTTPS e importar chaves GPG.

4. **Verifique se o WSL está na versão 2**
   ```bash
   wsl --set-default-version 2
   ```
   - *Explicação:* Usa as variáveis do sistema para identificar a distribuição e versão do Ubuntu.

5. **Carregue as variáveis do sistema**
   - Comando: `. /etc/os-release`
   - *Explicação:* Usa as variáveis do sistema para identificar a distribuição e versão do Ubuntu.

6. **Adicione a chave GPG oficial do Docker**
   ```bash
   curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc
   ```
   - *Explicação:* Garante a autenticidade dos pacotes baixados do repositório Docker.

6. **Adicione o repositório oficial do Docker**
   ```bash
   echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list
   ```
   - *Explicação:* Permite instalar o Docker diretamente do repositório oficial.

7. **Atualize a lista de pacotes novamente**
   ```bash
   sudo apt update
   ```
   - *Explicação:* Inclui os pacotes do novo repositório Docker.

8. **Instale o Docker**
   ```bash
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```
   - *Explicação:* Instala o Docker Engine e componentes necessários.

9. **Adicione seu usuário ao grupo docker**
   ```bash
   sudo usermod -aG docker $USER
   ```
   - *Explicação:* Permite rodar comandos Docker sem sudo.

10. **Atualize o grupo do usuário na sessão atual**
    ```bash
    newgrp docker
    ```
    - *Explicação:* Aplica imediatamente a permissão de uso do Docker sem precisar reiniciar.

---

## (Opcional) Instale o Portainer para gerenciamento visual de containers

```bash
sudo docker run -d -p 9000:9000 --name portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data portainer/portainer-ce
```
- *Explicação:* O Portainer oferece uma interface web para gerenciar containers Docker de forma simples e visual.

<!--
Arquivo: docker-portainer.md
Descrição: Guia para rodar o Portainer em um container no docker local.
-->

# Como rodar o Portainer em um container no docker local

Este tutorial ensina a rodar o Portainer em um container no docker local.

1. **Instale o Docker na distribuição Linux**
   - Siga o tutorial - [docker-install.md](docker-install.md)
   - *Explicação:* O Docker facilita a execução de ambientes isolados.

2. **Instale o Portainer para gerenciar containers**
   ```bash
   sudo docker run -d -p 9000:9000 --name portainer --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v portainer_data:/data portainer/portainer-ce
   ```

   - *Explicação:* O Portainer oferece uma interface web para gerenciar containers Docker.

3. **Acesse o Portainer**
   - Abra o navegador e acesse: `http://localhost:9000`
   - Crie um usuário administrador na primeira execução
   - *Explicação:* A interface web permite gerenciar containers visualmente.

4. **Gerencie containers através do Portainer**
   - **Ver containers:** Na seção "Containers" você pode ver todos os containers em execução
   - **Iniciar/Parar:** Use os botões de play/stop para controlar containers
   - **Ver logs:** Clique no container e depois em "Logs" para ver os logs em tempo real
   - **Remover containers:** Selecione containers e clique em "Remove"
   - *Explicação:* O Portainer facilita o gerenciamento sem precisar usar a linha de comando.

5. **Gerencie imagens Docker**
   - Na seção "Images" você pode:
     - Ver todas as imagens disponíveis
     - Baixar novas imagens (Pull)
     - Remover imagens não utilizadas
     - Construir imagens a partir de Dockerfiles
   - *Explicação:* Gerencie o repositório de imagens visualmente.

6. **Gerencie volumes e redes**
   - **Volumes:** Na seção "Volumes" você pode gerenciar dados persistentes
   - **Redes:** Na seção "Networks" você pode configurar redes Docker
   - *Explicação:* Controle os recursos de armazenamento e rede dos containers.

7. **Comandos úteis complementares**
   ```bash
   # Verificar se o Portainer está rodando
   sudo docker ps | grep portainer
   
   # Ver logs do Portainer
   sudo docker logs portainer
   
   # Parar o Portainer
   sudo docker stop portainer
   
   # Iniciar o Portainer
   sudo docker start portainer
   
   # Remover o Portainer (se necessário)
   sudo docker rm portainer
   sudo docker volume rm portainer_data
   ```

8. **Dicas de segurança**
   - Mantenha o Portainer atualizado:
     ```bash
     sudo docker pull portainer/portainer-ce
     ```
   - Não exponha o Portainer diretamente à internet sem autenticação
   - Use senhas fortes para o usuário administrador
   - Considere usar HTTPS em ambientes de produção

## Troubleshooting

### Problema: "Cannot connect to the Docker daemon"
**Solução:** Verifique se o Docker está rodando:
```bash
sudo systemctl status docker
sudo systemctl start docker
```

### Problema: Portainer não acessível em localhost:9000
**Solução:** Verifique se o container está rodando e a porta está correta:
```bash
sudo docker ps
sudo netstat -tlnp | grep 9000
```

### Problema: Permissão negada ao executar comandos Docker
**Solução:** Adicione seu usuário ao grupo docker ou use sudo:
```bash
sudo usermod -aG docker $USER
# Faça logout e login novamente
```

---

**Recursos úteis:**
- [Documentação oficial Portainer](https://docs.portainer.io/)
- [Docker Hub - Portainer](https://hub.docker.com/r/portainer/portainer-ce)
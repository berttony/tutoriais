sudo apt update && sudo apt upgrade

sudo apt remove docker docker-engine docker.io containerd runc

sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2

. /etc/os-release

curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc

echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

sudo usermod -aG docker $USER

newgrp docker

3 - (Opcional) já logado na distribuição linux, instale o Portainer para ter um gerenciamento visual de seus containers: 
    sudo docker run -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

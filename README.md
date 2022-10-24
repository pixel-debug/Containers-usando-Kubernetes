# Roteiro prático sobre Criação de Containers usando Kubernetes

Objetivo: colocar em prática os conceitos de criação de Containers usando Kubernetes.

<!--ts-->   
   * [Introdução ao Kubernetes](#introdução-ao-kubernetes)
     * [Arquitetura](#arquitetura-do-kubernetes)
     * [Vantagens](#vantagens) 
   * [Instalações necessárias](#primeiros-passos)  
   * [Mãos a obra](#mãos-a-obra)  
   * [Alunos](#realizado-por)
<!--te-->



# Introdução ao Kubernetes
## O que é e qual o objetivo do Kubernetes?
*Kubernetes* é um sistema de open source para orquestração de contêineres Docker, ou seja, é responsável pelo monitoramento de aplicações conteinerizadas. Seu principal objetivo é facilitar o monitoramento, abstraindo a complexidade que seria necessária com a criação de scripts para iniciar contêineres, monitorar e controlar o retorno de erros em caso de quedas ou falhas no sistema.

## Arquitetura do Kubernetes

![alt text](https://github.com/pixel-debug/Containers-usando-Kubernetes/blob/master/imagens/arquitetura "Arquitetura")

## Vantagens

- Fácil manipulação de contêineres, que na verdade ocorrem através dos objetos PODS;

- Escalonamento da a aplicação;

- Volumes persistentes;

- Regeneração própria;

- Abstração de complexidade.

# Primeiros Passos

> Rode os seguinte comandos no terminal para instalar a máquina virtual:
```
  sudo apt update

  sudo apt -y upgrade
  
  // O seguinte comando irá realizar uma reinicialização, você foi avisado
  
  [ -f /var/run/reboot-required ] && sudo reboot -f

  curl https://www.virtualbox.org/download/oracle_vbox_2016.asc | gpg --dearmor > oracle_vbox_2016.gpg
  
  curl https://www.virtualbox.org/download/oracle_vbox.asc | gpg --dearmor > oracle_vbox.gpg

  sudo install -o root -g root -m 644 oracle_vbox_2016.gpg /etc/apt/trusted.gpg.d/
  
  sudo install -o root -g root -m 644 oracle_vbox.gpg /etc/apt/trusted.gpg.d/
  
  echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list

  sudo apt update
  
  sudo apt install linux-headers-$(uname -r) dkms
  
  sudo apt install virtualbox-7.0
  
  wget https://download.virtualbox.org/virtualbox/7.0.0/Oracle_VM_VirtualBox_Extension_Pack-7.0.0.vbox-extpack
  
  sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-*.vbox-extpack


```

> Para conseguir utilizar o Kubernetes na máquina Linux, copie e cole os seguintes comandos no terminal:

1.Baixe a versão mais recente

``` 
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" 

```

2.Instalar kubectl

``` 
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl 

```

3.Teste para garantir que a versão que você instalou esteja atualizada

``` 
  kubectl version --client --output=yaml  

```

> Para facilitar o processamento do Kubernetes na máquina local, é recomendado o Minikube. 

Caso seu sistema operacional for Linux x86-64, a versão estável mais recente usando arquivo binário pode ser conseguida com o seguinte comando:

``` 
  kubectl version --client --output=yaml  

```


4. Instalando Minicube

> Primiera mente para instalar as dependencias do minicube use o comando:

``` 
  sudo apt install -y curl wget apt-transport-https

``` 

> Apos isso abaixe menicube com o comando:

``` 
  wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

``` 

> Apos isso mova o arquivo e de a ele as permisões necessarias com os comandos:

``` 
  sudo cp minikube-linux-amd64 /usr/local/bin/minikube
  sudo chmod +x /usr/local/bin/minikube

``` 

> Iniciando o minicube e fazendo as configurações iniciais com os comandos:

``` 
  minikube start --driver=docker
  minikube start --addons=ingress --cpus=2 --cni=flannel --install-addons=true --kubernetes-version=stable --memory=6g

``` 

>> Obs.: para outras arquiteturas por favor [acesse](https://minikube.sigs.k8s.io/docs/start/).


Crie um diretório para a aplicação e nele digite, com acesso de administrador (mas não logado como root):

``` 
  minikube start 

``` 

Esse comando irá criar uma máquina virtual, onde teremos um cluster para trabalhar com nossa aplicação.


# Mãos a Obra
Depois de tudo instalado e o diretório da aplicação criado, precisamos criar nosso arquivo com as configurações do objeto (POD).

```	
    Arquivo aplicacao.yaml

    kubectl create -f aplicacao.yaml 

```

Crie também o objeto deployment, ele será responsável por atualizar o Kubernetes sobre as informações de processamento.

``` 
  Arquivo deployment.yaml 
  
```

Isso significa que estamos colocando nosso objeto POD dentro do Deployment, que por sua vez possui mais recursos e conseguirá então, passar o estado desejado que foi configurado para nosso Kubernetes. É possível ver que o segundo _spec_ é basicamente o _spec_ do nosso objeto POD inicial. Agora basta rodar o comando para criar o objeto, em seguida, vamos listar ele:

``` 
    kubectl create -f deployment.yaml

    kubectl get pods 
```

Porém, ainda não é possível acessar a aplicação Web, uma vez que os objetos PODs são instáveis e sofrem constantes alterações. Para isso precisamos criar o objeto service. Ele irá funcionar como um balanceador de cargas e nos permitirá o acesso a aplicação.

``` 
  Arquivo servico-aplicacao.yaml 

```

Ok, agora podemos ter acesso ao URL da nossa aplicação

``` 
  kubectl create -f servico-aplicacao.yaml

  minikube service servico-aplicacao --url
```

Acessando a URL é possível perceber que o erro 403 Forbidden foi apresentado na tela. Isso acontece por que estamos usando como base a seguinte imagem ```php:5.6-apache```, que não vem com nada no diretório html. Agora só precisamos adicionar algum arquivo no objeto deployment para testar

```
  kubectl exec -it aplicacao-deployment-787789445d-5m6zz bash

  echo "<?php phpinfo(); ?>" > index.php
```

Pronto! Você está rodando uma aplicação Web com Kubernetes!

# Realizado por
1. Caio Lott

2. Marina Bernardes Diniz - 20193008242

3. Samuel Correa

4. Tomaz Augusto - 20193016389

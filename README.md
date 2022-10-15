# Roteiro prático sobre Criação de Containers usando Kubernetes

Objetivo: colocar em prática os conceitos de criação de Containers usando Kubernetes.

<!--ts-->   
   * [Introdução ao Kubernetes](#introdução-ao-kubernetes)
     * [Arquitetura](#arquitetura-do-kubernetes)
     * [Vantagens](#vantagens) 
   * [Instalações necessárias](#primeiros-passos)  
   * [Alunos](#realizado-por)
<!--te-->



# Introdução ao Kubernetes
## O que é e qual o objetivo do Kubernetes?
*Kubernetes* é um sistema de open source para orquestração de contêineres Docker, ou seja, é responsável pelo monitoramento de aplicações conteinerizadas. Seu principal objetivo é facilitar o monitoramento, abstraindo a complexidade que seria necessária com a criação de scripts para iniciar contêineres, monitorar e controlar o retorno de erros em caso de quedas ou falhas no sistema.

## Arquitetura do Kubernetes

![alt text](https://github.com/pixel-debug/Containers-usando-Kubernetes/blob/master/imagens/arquitetura "Arquitetura")

## Vantagens

# Primeiros Passos
Para conseguir utilizar o Kubernetes na máquina Linux, copie e cole os seguintes comandos no terminal:

1.Baixe a versão mais recente

``` curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" ```

2.Instalar kubectl

``` sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl ```

3.Teste para garantir que a versão que você instalou esteja atualizada

``` kubectl version --client --output=yaml  ```

Para facilitar o processamento do Kubernetes na máquina local, é recomendado o Minikube. 

Caso seu sistema operacional for Linux x86-64, a versão estável mais recente usando arquivo binário pode ser conseguida com o seguinte comando:

``` kubectl version --client --output=yaml  ```

Obs.: para outras arquiteturas por favor [acesse](https://minikube.sigs.k8s.io/docs/start/).


A partir de um terminal com acesso de administrador (mas não logado como root), execute:

``` minikube start ``` 


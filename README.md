# Kubernete the Morais way

Aqui você encontra uma série de playbooks para criação de um cluster kubernetes local.

Este cluster foi criado com base no guia:
[Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)

## Pré requisitos:

1. Ter o Virtual Box na última versão.
2. Ter o Vagrant na última versão.
3. Ter o plugin dotenv do Vagrant.  

## Detalhes do cluster

### Versões

1. Vagrant Box: bento/centos-8.1 (202005.21.0)

## Como subir o cluster

1. Clone este repositório
2. Efetue uma cópia do `.env-sample`:  
    ```bash
    cp .env-sample .env
    ```
3. Execute o comando:
    ```bash
    vagrant up
    ```
    
# Referências

- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- [Vagrant](https://www.vagrantup.com/docs)
- [Vagrant Env](https://github.com/gosuri/vagrant-env)
- [Box Centos 8.1](https://app.vagrantup.com/bento/boxes/centos-8.1)

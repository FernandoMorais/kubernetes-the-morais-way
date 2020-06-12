# Kubernete the Morais way

Aqui você encontra uma série de playbooks para criação de um cluster kubernetes local.

Este cluster foi criado com base no guia:
[Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)

## Pré requisitos:

1. Ter o Virtual Box na última versão *.
    * para usuários Ubuntu utilize o Virtual Box 6.0!
2. Ter o Vagrant na última versão.
3. Ter o plugin dotenv do Vagrant.  
4. Ter o ansible na versão 2.8

## Detalhes do cluster

### Versões

1. Vagrant Box: bento/centos-8.1 (202005.21.0)
2. cfssl (1.3.4)
3. cfssljson (1.3.4)
5. kubectl (v1.15.3)

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
- [Vagrant Provision Ansible](https://www.vagrantup.com/docs/provisioning/ansible.html)
- [Box Centos 8.1](https://app.vagrantup.com/bento/boxes/centos-8.1)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Jinja Template](https://jinja.palletsprojects.com/en/2.11.x/)
- [Public Key Infrastructure @ Wikipedia](https://en.wikipedia.org/wiki/Public_key_infrastructure)
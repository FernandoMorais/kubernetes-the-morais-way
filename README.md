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
5. Ter o git na última versão.

## Detalhes do cluster

### Versões

1. Vagrant Box: bento/centos-8.1 (202005.21.0)
2. cfssl (1.3.4)
3. cfssljson (1.3.4)
5. kubectl (v1.15.3)

## Como começar

## Clone do repositório

1. Clone este repositório

    ```bash
    git clone git@github.com:FernandoMorais/kubernetes-the-morais-way.git
    cd kubernetes-the-morais-way
    ```

## Parametrização

1. Efetue uma cópia do `.env-sample`:  
    ```bash
    cp .env-sample .env
    ```

    |Variável|Padrão|Descrição  
    |-|-|-|  
    |K8S_CONTROLLER_IP|192.168.50.100|  
    |K8S_CONTROLLER_CPU|2|  
    |K8S_CONTROLLER_RAM|2048|  
    |K8S_WORKER_1_IP|192.168.50.101|  
    |K8S_WORKER_1_CPU|2|  
    |K8S_WORKER_1_RAM|2048|  
    |K8S_CA_C|BR|(*)  
    |K8S_CA_L|Jundiai|(*)
    |K8S_CA_O|Kubernetes|(*)
    |K8S_CA_OU|CA|(*)
    |K8S_CA_ST|Sao Paulo|(*)
    |K8S_CA_EXPIRY|8760h|(*)

    (*) Os parâmetros K8S_CA_\* não podem conter acentos ou caracteres especiais!

## Como subir o cluster

1. Execute o comando:
    ```bash
    vagrant up
    ```
    
## Os playbooks

|Playbook|Descrição
|-|-
|step-00-test|  
|step-01-setup-tools|  
|step-02-setup-certificates|  
|step-03-setup-kubeconfig|  
|step-04-setup-encryption|  
|step-05-setup-etcd|  

# Referências

- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- [Vagrant](https://www.vagrantup.com/docs)
- [Vagrant Env](https://github.com/gosuri/vagrant-env)
- [Vagrant Provision Ansible](https://www.vagrantup.com/docs/provisioning/ansible.html)
- [Box Centos 8.1](https://app.vagrantup.com/bento/boxes/centos-8.1)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Jinja Template](https://jinja.palletsprojects.com/en/2.11.x/)
- [Public Key Infrastructure @ Wikipedia](https://en.wikipedia.org/wiki/Public_key_infrastructure)
- [Encrypting Secret Data at Rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
- [Etcd](https://etcd.io/)
- [Raft](https://raft.github.io/)
require 'dotenv/load'

def to_boolean(s)
    s.to_s.downcase == "true"
end

vms = {
    "k8s-controller"   => {
        :role => "controller",
        :vm_box => "bento/centos-8.1", 
        :vm_box_version => "202005.21.0", 
        :vm_cpu => ENV['K8S_CONTROLLER_CPU'], 
        :vm_ram => ENV['K8S_CONTROLLER_RAM'], 
        :vm_ip => ENV['K8S_CONTROLLER_IP']
    },
    "k8s-worker-1" => {
        :role => "worker",
        :index => 0,
        :enabled => true,
        :vm_box => "bento/centos-8.1",
        :vm_box_version => "202005.21.0",
        :vm_cpu => ENV['K8S_WORKER_1_CPU'],
        :vm_ram => ENV['K8S_WORKER_1_RAM'],
        :vm_ip => ENV['K8S_WORKER_1_IP']
    },
    "k8s-worker-2" => {
        :role => "worker",
        :index => 1,
        :enabled => ENV['K8S_WORKER_2_ENABLED'],
        :vm_box => "bento/centos-8.1",
        :vm_box_version => "202005.21.0",
        :vm_cpu => ENV['K8S_WORKER_2_CPU'],
        :vm_ram => ENV['K8S_WORKER_2_RAM'],
        :vm_ip => ENV['K8S_WORKER_2_IP']
    }
}

Vagrant.configure("2") do |config|
    vms.each do | vm_name, vm_params |
        if vm_params[:role] == 'controller' || to_boolean(vm_params[:enabled])
            config.vm.define "#{vm_name}" do |vm_item|

                vm_item.vm.hostname    = "#{vm_name}"
                vm_item.vm.box         = "#{vm_params[:vm_box]}"
                vm_item.vm.box_version = "#{vm_params[:vm_box_version]}"

                vm_item.vm.network "private_network", ip: "#{vm_params[:vm_ip]}"

                vm_item.vm.provider "virtualbox" do |vm_item_vb|

                    vm_item_vb.cpus   = vm_params[:vm_cpu]
                    vm_item_vb.memory = vm_params[:vm_ram]

                end
            end
        end
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-01-setup-tools.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"]
        }
        ansible.extra_vars = {}
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-02-setup-certificates.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"],
            "controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| k },
            "worker_nodes" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| k },
        }
        ansible.extra_vars = {
            "kubernetes_controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
            "kubernetes_worker_nodes" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
            "certificates_c" => ENV['K8S_CA_C'],
            "certificates_l" => ENV['K8S_CA_L'],
            "certificates_o" => ENV['K8S_CA_O'],
            "certificates_ou" => ENV['K8S_CA_OU'],
            "certificates_st" => ENV['K8S_CA_ST'],
            "certificates_ca_expiry" => ENV['K8S_CA_EXPIRY']
        }
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-03-setup-kubeconfig.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"],
            "controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| k },
            "worker_nodes" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled]) }.map{ |k,v| k },
        }
        ansible.extra_vars = {
            "kubernetes_cluster_name" => ENV['K8S_CLUSTER_NAME'],
            "kubernetes_controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
            "kubernetes_worker_nodes" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
        }
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-04-setup-encryption.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"],
            "controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| k },
        }
        ansible.extra_vars = {}
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-05-setup-etcd.yml"
        ansible.become = true
        ansible.groups = {
            "controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| k },
        }
        ansible.extra_vars = {
            "kubernetes_controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| {:host => k, :ip => v[:vm_ip]} }
        }
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-06-bootstrap-control-plane.yml"
        ansible.become = true
        ansible.groups = {
            "controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| k },
        }
        ansible.extra_vars = {
            "kubernetes_cluster_name" => ENV['K8S_CLUSTER_NAME'],
            "kubernetes_controller_nodes" => vms.select{ |k,v| v[:role] =~ /controller/ }.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
            "kubernetes_all_nodes" => vms.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
        }
        ansible.tags = "vagrant"
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-07-bootstrap-worker-nodes.yml"
        ansible.become = true
        ansible.groups = {
            "worker_nodes" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| k },
        }
        ansible.extra_vars = {
            "kubernetes_worker_nodes" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| { :index => v[:index], :host => k, :ip => v[:vm_ip]} },
            "kubernetes_all_nodes" => vms.map{ |k,v| {:host => k, :ip => v[:vm_ip]} },
        }
        ansible.tags = "vagrant"
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-08-setup-dns-cluster-add-on.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"],
        }
        ansible.extra_vars = {}
    end

    ingress_ip = vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| v[:vm_ip] }[0]

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-09-bootstrap-ingress.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"],
        }
        ansible.extra_vars = {
            "ingress_traefik_externalIPs" => vms.select{ |k,v| v[:role] =~ /worker/ && to_boolean(v[:enabled])  }.map{ |k,v| v[:vm_ip] },
            "ingress_traefik_host" => "traefik.#{ingress_ip}.xip.io",
        }
        ansible.tags = "traefik"
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/step-10-bootstrap-netdata.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-controller"],
        }
        ansible.extra_vars = {}
    end
end

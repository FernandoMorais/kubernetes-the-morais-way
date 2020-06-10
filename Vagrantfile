require 'dotenv/load'

vms = {
    "k8s-master"   => {
        :vm_box => "bento/centos-8.1", 
        :vm_box_version => "202005.21.0", 
        :vm_cpu => ENV['K8S_MASTER_CPU'], 
        :vm_ram => ENV['K8S_MASTER_RAM'], 
        :vm_ip => ENV['K8S_MASTER_IP']
    },
    "k8s-worker-1" => {
        :vm_box => "bento/centos-8.1",
        :vm_box_version => "202005.21.0",
        :vm_cpu => ENV['K8S_WORKER_1_CPU'],
        :vm_ram => ENV['K8S_WORKER_1_RAM'],
        :vm_ip => ENV['K8S_WORKER_1_IP']
    }
}

Vagrant.configure("2") do |config|
    vms.each do | vm_name, vm_params |

        config.vm.define "#{vm_name}" do |vm_item|

            vm_item.vm.hostname    = "#{vm_name}"
            vm_item.vm.box         = "#{vm_params[:vm_box]}"
            vm_item.vm.box_version = "#{vm_params[:vm_box_version]}"

            vm_item.vm.network "private_network", ip: "#{vm_params[:vm_ip]}",
                virtualbox__intnet: true

            vm_item.vm.provider "virtualbox" do |vm_item_vb|

                vm_item_vb.cpus   = vm_params[:vm_cpu]
                vm_item_vb.memory = vm_params[:vm_ram]

            end
        end
    end

    config.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbooks/setup-tools.yml"
        ansible.become = true
        ansible.groups = {
            "setup_node" => ["k8s-master"]
        }
        ansible.extra_vars = {
            "kubectl_version" => "1.15.3"
        }
    end
end

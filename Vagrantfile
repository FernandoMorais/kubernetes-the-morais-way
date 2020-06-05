require 'dotenv/load'

vms = {
    "k8s-master"   => ["bento/centos-8.1", "202005.21.0", ENV['K8S_MASTER_CPU']  , ENV['K8S_MASTER_RAM']  , ENV['K8S_MASTER_IP']  ],
    "k8s-worker-1" => ["bento/centos-8.1", "202005.21.0", ENV['K8S_WORKER_1_CPU'], ENV['K8S_WORKER_1_RAM'], ENV['K8S_WORKER_1_IP']]
}

Vagrant.configure("2") do |config|
    vms.each do | vm_name, vm_params |

        vm_box, vm_box_version, vm_cpu, vm_ram, vm_ip = vm_params

        config.vm.define "#{vm_name}" do |vm_item|

            vm_item.vm.hostname    = "#{vm_name}"
            vm_item.vm.box         = "#{vm_box}"
            vm_item.vm.box_version = "#{vm_box_version}"

            vm_item.vm.network "private_network", ip: "#{vm_ip}",
                virtualbox__intnet: true

            vm_item.vm.provider "virtualbox" do |vm_item_vb|

                vm_item_vb.cpus   = vm_cpu   
                vm_item_vb.memory = vm_ram

            end

        end
    end
end

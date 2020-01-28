# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "512"
    vb.cpus = 2 # not less than the required 2
  end

  config.vm.define "k8s-master01" do |k8scluster|
      k8scluster.vm.box = "bento/ubuntu-19.04"
      k8scluster.vm.hostname = "k8s-master01"
      k8scluster.vm.network "private_network", ip: "10.217.50.10"
      k8scluster.vm.provider "virtualbox" do |vb|
          vb.name = "k8s-master01"
          vb.cpus = 3
          vb.memory = "6144"
      end
      k8scluster.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "provisioning/deploy.yml"
        ansible.become = true
        ansible.compatibility_mode = "2.0"
        ansible.version = "2.9.4" # ubuntu-19.04
      end
      k8scluster.vm.provision "shell", inline: <<-SHELL
      echo "===================================================================================="
                    hostnamectl status
      echo "===================================================================================="
      echo "         \   ^__^                                                                  "
      echo "          \  (oo)\_______                                                          "
      echo "             (__)\       )\/\                                                      "
      echo "                 ||----w |                                                         "
      echo "                 ||     ||                                                         "
      SHELL
    end

    config.vm.define "worker01" do |k8scluster|
        k8scluster.vm.box = "bento/ubuntu-19.04"  # OK Kernel: Linux 5.0.0-17-generic
        # k8scluster.vm.box = "bento/ubuntu-19.04" # NOT OK. minimal supported kernel version is >= 4.8.0;
        k8scluster.vm.hostname = "worker01"
        k8scluster.vm.network "private_network", ip: "10.217.50.11"
        k8scluster.vm.provider "virtualbox" do |vb|
            vb.name = "worker01"
            vb.memory = "1024"
        end
        k8scluster.vm.provision "ansible_local" do |ansible|
          ansible.playbook = "provisioning/deploy.yml"
          ansible.become = true
          ansible.compatibility_mode = "2.0"
          ansible.version = "2.9.4"
        end
        k8scluster.vm.provision "shell", inline: <<-SHELL
        echo "===================================================================================="
                      hostnamectl status
        echo "===================================================================================="
        echo "         \   ^__^                                                                  "
        echo "          \  (oo)\_______                                                          "
        echo "             (__)\       )\/\                                                      "
        echo "                 ||----w |                                                         "
        echo "                 ||     ||                                                         "
        SHELL
      end


      config.vm.define "worker02" do |k8scluster|
          k8scluster.vm.box = "bento/ubuntu-19.04"  # OK Kernel: Linux 5.0.0-17-generic
          # k8scluster.vm.box = "bento/ubuntu-19.04" # NOT OK. minimal supported kernel version is >= 4.8.0;
          k8scluster.vm.hostname = "worker02"
          k8scluster.vm.network "private_network", ip: "10.217.50.12"
          k8scluster.vm.provider "virtualbox" do |vb|
              vb.name = "worker02"
              vb.memory = "1024"
          end
          k8scluster.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "provisioning/deploy.yml"
            ansible.become = true
            ansible.compatibility_mode = "2.0"
            ansible.version = "2.9.4"
          end
          k8scluster.vm.provision "shell", inline: <<-SHELL
          echo "===================================================================================="
                        hostnamectl status
          echo "===================================================================================="
          echo "         \   ^__^                                                                  "
          echo "          \  (oo)\_______                                                          "
          echo "             (__)\       )\/\                                                      "
          echo "                 ||----w |                                                         "
          echo "                 ||     ||                                                         "
          SHELL
        end

        config.vm.define "remotecontrol01" do |k8scluster|
            k8scluster.vm.box = "bento/centos-7.7" #  KERNEL ??? Kernel: Linux 3.10.0-957.1.3.el7.x86_64
            k8scluster.vm.hostname = "remotecontrol01"
            k8scluster.vm.network "private_network", ip: "10.217.50.13"
            k8scluster.vm.provider "virtualbox" do |vb|
                vb.name = "remotecontrol01"
            end
            k8scluster.vm.provision "ansible_local" do |ansible|
              ansible.playbook = "provisioning/deploy.yml"
              ansible.become = true
              ansible.compatibility_mode = "2.0"
              ansible.version = "2.9.2"
            end
            k8scluster.vm.provision "shell", inline: <<-SHELL
            yum install -y python3 # centos specific
            echo "===================================================================================="
                          hostnamectl status
            echo "===================================================================================="
            echo "         \   ^__^                                                                  "
            echo "          \  (oo)\_______                                                          "
            echo "             (__)\       )\/\                                                      "
            echo "                 ||----w |                                                         "
            echo "                 ||     ||                                                         "
            SHELL
          end
end

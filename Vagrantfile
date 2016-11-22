Vagrant.configure(2) do |config|
  config.vm.provider "libvirt" do |libvirt, override|
    libvirt.driver = "kvm"
    libvirt.memory = 4096
    libvirt.cpus = 2
  end

  config.vm.define :sqlserver_tester do |vagrant_host|
    # Box name
    vagrant_host.vm.box = "fedora/24-cloud-base"
    vagrant_host.vm.hostname = "sqlserver-tester"
    
    config.vm.synced_folder ".", "/vagrant/sync", type: "rsync",
                            rsync__exclude: [ ".git/", ".#*", "*~" ]
    
    vagrant_host.vm.provision 'shell', inline: "dnf install -y docker"
    vagrant_host.vm.provision 'shell', inline: "sudo systemctl stop docker > /dev/null 2>&1 || :" #in case this isn't first run
    vagrant_host.vm.provision 'shell', inline: "sudo groupadd docker > /dev/null 2>&1 || :"
    vagrant_host.vm.provision 'shell', inline: "sudo usermod -a -G docker vagrant"
    vagrant_host.vm.provision 'shell', inline: "sudo systemctl enable docker && sudo systemctl start docker"
    vagrant_host.vm.provision 'shell', inline: "sudo chown root:docker /var/run/docker.sock"

    vagrant_host.vm.provision 'shell', inline: "dnf install -y atomic"

    vagrant_host.vm.provision 'shell', inline: "mkdir -p /mnt/msql-data/data"
    vagrant_host.vm.provision 'shell', inline: "mkdir -p /mnt/msql-data/log"

    #vagrant_host.vm.provision 'shell', inline: "cd /vagrant/sync/src/ && ./runit.sh"
  end

end

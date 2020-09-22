Dotenv.load

Vagrant.configure(2) do |config|

  config.vm.box = "debian/buster64"

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
    v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root","1"]
  end

  config.vm.network "private_network", type: "dhcp", ip: "192.168.33.0"

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
    ip_address = ''
    if hostname = (vm.ssh_info && vm.ssh_info[:host])
      vm.communicate.execute("/bin/ip addr show dev eth1 | grep 'inet ' | tail -n 1 | egrep -o '[0-9\.]+' | head -n 1 2>&1") do |type, contents|
        ip_address = contents.split("\n").first
      end
    end
    ip_address
  end

  config.hostmanager.aliases = ["#{ENV['VAGRANT_HOST']}"]

  config.vm.synced_folder ".", "/vagrant", type:"virtualbox", create: true

  config.vm.provision :docker
  config.vm.provision :docker_compose

  # Docker SSH Connection
  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    sed -i -e 's|^#\\?MaxSessions.*|MaxSessions 100|' /etc/ssh/sshd_config
    /etc/init.d/ssh restart
  SHELL

  # VS CODE settings.json docker.host
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    FILE="/vagrant/.vscode/settings.json"
    if test -f $FILE;
    then
      sed -i -e 's|\\(//\\)\\?\\s\\?"docker.host":.*|"docker.host": "ssh://vagrant@#{ENV['VAGRANT_HOST']}",|' $FILE
    fi
  SHELL
end

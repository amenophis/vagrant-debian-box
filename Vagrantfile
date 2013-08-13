personalization = File.expand_path("../config", __FILE__)
load personalization

Vagrant.configure("2") do |config|
  config.vm.box = $box_name
  config.vm.box_url = $box_url

  config.vm.network :private_network, ip: $ip
    config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--memory", $ram]
    v.customize ["modifyvm", :id, "--name", $name]
  end

  nfs_setting = RUBY_PLATFORM =~ /darwin/ || RUBY_PLATFORM =~ /linux/
  config.vm.synced_folder "../", $remote_folder, id: "vagrant-root", :nfs => $nfs

  config.vm.provision :shell, :inline => "sudo apt-get update"


  config.vm.provision :shell, :inline => 'echo -e "mysql_root_password=root
controluser_password=awesome" > /etc/phpmyadmin.facts;'

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    #puppet.options = ['--verbose']
    puppet.facter = $puppet_facter
  end
end

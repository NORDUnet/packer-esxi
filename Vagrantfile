Vagrant.configure('2') do |config|
  config.ssh.username = "root"
  config.ssh.insert_key = true
  config.ssh.shell = "/bin/sh"
  config.vm.provider "vmware_fusion" do |v|
    v.gui = true
  end
end

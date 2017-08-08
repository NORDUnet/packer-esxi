
$apiKey = SecureRandom.hex(64)
$harden = <<SCRIPT
sed -i '/pam_passwdqc.so/ s/$/ enforce=users/' /etc/pam.d/passwd
echo "#{$apiKey}" | tee /apikey | passwd root --stdin
echo "ChallengeResponseAuthentication no" >> /etc/ssh/sshd_config
/etc/init.d/SSH restart
SCRIPT

Vagrant.configure('2') do |config|
  config.ssh.username = "root"
  config.ssh.insert_key = true
  config.ssh.shell = "/bin/sh"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider "vmware_fusion" do |v|
    v.gui = true
  end
  config.vm.provision "shell", privileged: false, inline: $harden
end

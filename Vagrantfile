require 'securerandom'

$apiKey = SecureRandom.hex(32)
$harden = <<SCRIPT
sed -i '/pam_passwdqc.so/ s/$/ enforce=users/' /etc/pam.d/passwd
echo "#{$apiKey}" | passwd --stdin
sed -i '/^exit 0/iAPIKEY=#{$apiKey}' /etc/rc.local.d/local.sh
echo "ChallengeResponseAuthentication no" >> /etc/ssh/sshd_config
/etc/init.d/SSH restart
/bin/auto-backup.sh
SCRIPT

Vagrant.configure('2') do |config|
  config.ssh.username = "root"
  config.ssh.insert_key = true
  config.ssh.shell = "/bin/sh"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider "vmware_fusion" do |v|
    v.gui = true
  end
  config.vim.provision "shell", privileged: false, inline: <<-SHELL
    DATASTORE=/dev/disks/mpx.vmhba1\:C0\:T1\:L0
    partedUtil mklabel $DATASTORE gpt
    LASTSECTOR=$(partedUtil getUsableSectors $DATASTORE | cut -f2 -d' ')
    partedUtil setptbl $DATASTORE gpt "1 2048 $LASTSECTOR AA31E02A400F11DB9590000C2911D1B8 0"
    vmkfstools -C vmfs5 -b 1m -S NIAB_datastore ${DATASTORE}:1
  SHELL
  config.vm.provision "shell", privileged: false, inline: $harden
end

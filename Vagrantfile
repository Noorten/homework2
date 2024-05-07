MACHINES = {
  :"kernel-update" => {
              :box_name => "generic/centos8s",
              :box_version => "4.3.4",
              :cpus => 2,
              :memory => 1024,
            }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s
      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end

      # Провижин для обновления ядра
      box.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
        sudo yum --enablerepo elrepo-kernel install kernel-ml -y
        sudo grub2-mkconfig -o /boot/grub2/grub.cfg
        sudo grub2-set-default 0
        sudo reboot
      SHELL
    end
  end
end

# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :otus => {
        :box_name => "centos/7",

        :ip_addr => '192.168.11.101',
  },

}

Vagrant.configure("2") do |config|

	config.vm.box_version = "1804.02"
		MACHINES.each do |boxname, boxconfig|
		        config.vbguest.no_install = true

			config.vm.define boxname do |box|
			box.vm.box = boxconfig[:box_name]
			box.vm.host_name = boxname.to_s
			#box.vm.network "forwarded_port", guest: 3260, host: 3260+offset
			box.vm.network "private_network", ip: boxconfig[:ip_addr]
			box.vm.provider :virtualbox do |vb|
				vb.customize ["modifyvm", :id, "--memory", "1024"]
               		end
			box.vm.provision "shell", inline: <<-SHELL
				mkdir -p ~root/.ssh
				cp ~vagrant/.ssh/auth* ~root/.ssh
 ###Домашнее задание Авторизация и аутентификация
        #Запретить всем пользователям, кроме группы admin логин в выходные (суббота и воскресенье), без учета праздников
        #Добавим группу и пользователей:
        groupadd admin
        useradd admin1 && echo "pass"|passwd --stdin admin1 && usermod -a -G admin admin1; \
        useradd notadmin1 && echo "pass"|passwd --stdin notadmin1
        #Добавим правила pam
        echo '*;*;!vagrant;!Wd0000-2400' >> /etc/security/time.conf
        sed -i '4iaccount [success=1 default=ignore] pam_succeed_if.so user ingroup admin' /etc/pam.d/system-auth
        sed -i '5iaccount required pam_time.so' /etc/pam.d/system-auth
        #Задание без звездочек выполнено


			SHELL
			end
		end
end
	

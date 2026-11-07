Vagrant.configure("2") do |config|

  # VICTIM - Ubuntu with Apache 2.4.49 (VULNERABLE)
  config.vm.define "victim" do |victim|
    victim.vm.box = "ubuntu/focal64"
    victim.vm.network "private_network", ip: "192.168.56.10"
    victim.vm.hostname = "victim"
    victim.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.name = "CVE_victim"
    end
    victim.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y build-essential libpcre3 libpcre3-dev libssl-dev libexpat1-dev wget
      cd /tmp
      wget -q https://archive.apache.org/dist/httpd/httpd-2.4.49.tar.gz
      wget -q https://archive.apache.org/dist/apr/apr-1.7.0.tar.gz
      wget -q https://archive.apache.org/dist/apr/apr-util-1.6.1.tar.gz
      tar -xzf httpd-2.4.49.tar.gz
      tar -xzf apr-1.7.0.tar.gz
      tar -xzf apr-util-1.6.1.tar.gz
      mv apr-1.7.0 httpd-2.4.49/srclib/apr
      mv apr-util-1.6.1 httpd-2.4.49/srclib/apr-util
      cd httpd-2.4.49
      ./configure --enable-so --enable-cgi --with-included-apr > /dev/null 2>&1
      make > /dev/null 2>&1
      make install > /dev/null 2>&1
      sed -i 's/Require all denied/Require all granted/' /usr/local/apache2/conf/httpd.conf
      sed -i 's/#LoadModule cgid_module/LoadModule cgid_module/' /usr/local/apache2/conf/httpd.conf
      # Auto-start Apache on boot
      cat > /etc/systemd/system/apache249.service <<EOF
[Unit]
Description=Apache 2.4.49
After=network.target

[Service]
ExecStart=/usr/local/apache2/bin/apachectl start
ExecStop=/usr/local/apache2/bin/apachectl stop
Type=forking

[Install]
WantedBy=multi-user.target
EOF
      systemctl enable apache249
      systemctl start apache249
    SHELL
  end

  # VICTIM PATCHED - Ubuntu with Apache 2.4.51 (PATCHED)
  config.vm.define "victim_patched" do |vp|
    vp.vm.box = "ubuntu/focal64"
    vp.vm.network "private_network", ip: "192.168.56.30"
    vp.vm.hostname = "victim-patched"
    vp.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.name = "CVE_victim_patched"
    end
    vp.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y build-essential libpcre3 libpcre3-dev libssl-dev libexpat1-dev wget
      cd /tmp
      wget -q https://archive.apache.org/dist/httpd/httpd-2.4.51.tar.gz
      wget -q https://archive.apache.org/dist/apr/apr-1.7.0.tar.gz
      wget -q https://archive.apache.org/dist/apr/apr-util-1.6.1.tar.gz
      tar -xzf httpd-2.4.51.tar.gz
      tar -xzf apr-1.7.0.tar.gz
      tar -xzf apr-util-1.6.1.tar.gz
      mv apr-1.7.0 httpd-2.4.51/srclib/apr
      mv apr-util-1.6.1 httpd-2.4.51/srclib/apr-util
      cd httpd-2.4.51
      ./configure --enable-so --enable-cgi --with-included-apr > /dev/null 2>&1
      make > /dev/null 2>&1
      make install > /dev/null 2>&1
      sed -i 's/Require all denied/Require all granted/' /usr/local/apache2/conf/httpd.conf
      sed -i 's/#LoadModule cgid_module/LoadModule cgid_module/' /usr/local/apache2/conf/httpd.conf
      # Auto-start Apache on boot
      cat > /etc/systemd/system/apache251.service <<EOF
[Unit]
Description=Apache 2.4.51
After=network.target

[Service]
ExecStart=/usr/local/apache2/bin/apachectl start
ExecStop=/usr/local/apache2/bin/apachectl stop
Type=forking

[Install]
WantedBy=multi-user.target
EOF
      systemctl enable apache251
      systemctl start apache251
    SHELL
  end

  # ATTACKER - Kali
  config.vm.define "attacker" do |attacker|
    attacker.vm.box = "kalilinux/rolling"
    attacker.vm.network "private_network", ip: "192.168.56.20"
    attacker.vm.hostname = "attacker"
    attacker.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.name = "CVE_attacker"
    end
  end

end
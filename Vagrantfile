# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.hostname = "jenkins-centos"

  config.vm.network "public_network"

  config.vm.synced_folder "./", "/vagrant", type: "virtualbox", disabled: false

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
  end

  config.vm.provision "docker"

  config.vm.provision "shell", inline: <<-SHELL
    yum -y update
    yum -y install wget git
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    yum -y install java-1.8.0-openjdk-devel
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    echo 'export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk' | tee -a /etc/profile
    echo 'export JRE_HOME=/usr/lib/jvm/jre' | tee -a /etc/profile
    source /etc/profile
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
    yum -y install jenkins
    systemctl start jenkins.service
    systemctl enable jenkins.service
    usermod -aG docker jenkins
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    wget http://www-eu.apache.org/dist/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.tar.gz
    tar -zxvf apache-maven-3.5.3-bin.tar.gz
    mv apache-maven-3.5.3 /opt/
    chown -R root:root /opt/apache-maven-3.5.3
    ln -s /opt/apache-maven-3.5.3 /opt/apache-maven
    ln -s /opt/apache-maven-3.5.3/bin/mvn /bin/mvn
    echo 'export PATH=$PATH:/opt/apache-maven/bin' | tee -a /etc/profile
    source /etc/profile
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    rpm --nosignature -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
    yum -y install libunwind libicu
    yum -y install dotnet-sdk-2.1.200
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    curl -L https://bootstrap.saltstack.com -o install-salt.sh
    chmod +x install-salt.sh
    ./install-salt.sh -M -A 127.0.0.1
  SHELL
end

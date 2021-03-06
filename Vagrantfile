Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"

    config.vm.provision :shell, :inline => $PROVISION_SCRIPT

    config.vm.synced_folder "./mirror", "/home/vagrant/host-mirror"

    config.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
end


$PROVISION_SCRIPT = <<EOF
    if [ -f "/var/vagrant_provision" ]; then
        exit 0
    fi

    # Notice that `sudo` is used here throughout

    set -x -e     # verbose output, stop on any error
   
    sudo apt-get install build-essential software-properties-common -y 
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update -y

    sudo apt-get install -y gcc-9 > /dev/null 2>&1
    sudo apt-get install -y g++-9 > /dev/null 2>&1

    sudo apt-get install -y valgrind > /dev/null 2>&1
    sudo apt-get install -y gdb > /dev/null 2>&1

    sudo apt-get install -y python3.6 > /dev/null 2>&1
    sudo apt-get install -y python-pytest > /dev/null 2>&1
    sudo apt-get install -y tree > /dev/null 2>&1
    sudo apt-get install -y unzip > /dev/null 2>&1

    sudo update-alternatives --install /usr/bin/python \
        python /usr/bin/python2.7 10
    sudo update-alternatives --install /usr/bin/python \
        python /usr/bin/python3.6 20

    sudo update-alternatives --install /usr/bin/gcc \
        gcc /usr/bin/gcc-7 20
    sudo update-alternatives --install /usr/bin/gcc \
        gcc /usr/bin/gcc-9 30

    touch /var/vagrant_provision
EOF


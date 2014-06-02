# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Silly but effective syntax for declaring box hostname
  config.vm.define :srilmbox do |t|
  end

  # Canonical builds nightly Vagrant images for Ubuntu: http://cloud-images.ubuntu.com/vagrant/
  config.vm.box = "trusty32"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-i386-vagrant-disk1.box"

#  config.vm.network :forwarded_port, guest: 8000, host: 8000
#  config.vm.network :private_network, ip: "192.168.33.33"
#

  config.vm.provider :virtualbox do |vb, override|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.name = "srilmbox"
  end

  # config.vm.synced_folder "../data", "/vagrant_data"
  $setup_srilm = <<SCRIPT
  locale-gen en_US.UTF-8

  aptitude install -y build-essential python-dev swig git tcl tcl-dev

  export SWIGSRILM='/opt/swig-srilm'
  git clone https://github.com/desilinguist/swig-srilm $SWIGSRILM

  export SRILM='/opt/srilm'
  export NO_TCL=X
  export TCL_INCLUDE=''
  export TCL_LIBRARY=''


  mkdir -p $SRILM
  tar -xz --directory $SRILM -f /vagrant/provisioning/srilm.tgz

  make -C $SRILM

  perl -p -i -e 's#/home/speech/Resources/Tools/(srilm/lib/i686)-m64#/opt/$1#' $SWIGSRILM/Makefile
  perl -p -i -e 's#/home/speech/Resources/Tools/(srilm/include)#/opt/$1#' $SWIGSRILM/Makefile
  perl -p -i -e 's#/opt/python/2.7/include/python2.7#/usr/include/python2.7#' $SWIGSRILM/Makefile

  make -C $SWIGSRILM python

SCRIPT

    config.vm.provision "shell", inline: $setup_srilm

end

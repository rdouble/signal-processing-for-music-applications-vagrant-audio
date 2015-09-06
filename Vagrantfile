# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  # allow X-Forwarding
  config.ssh.forward_x11 = true

  config.vm.provision :shell, :inline => $BOOTSTRAP_SCRIPT # see below
  config.vm.provider :virtualbox do |vb|
    if OS.mac?
      vb.customize ["modifyvm", :id, '--audio', 'coreaudio', '--audiocontroller', 'hda'] # choices: hda sb16 ac97
    elsif OS.linux?
      vb.customize ["modifyvm", :id, '--audio', 'pulse', '--audiocontroller', 'hda'] # choices: hda sb16 ac97
    elsif OS.windows?
      vb.customize ["modifyvm", :id, '--audio', 'dsound', '--audiocontroller', 'hda'] # choices: hda sb16 ac97
    end

    vb.customize ["modifyvm", :id, "--vram", "9"] # prevent a warning
  end
  
end

$BOOTSTRAP_SCRIPT = <<EOF
  set -e # Stop on any error

  # --------------- SETTINGS ----------------
  # Other settings
  export DEBIAN_FRONTEND=noninteractive

  sudo apt-get update

  # ---- OSS AUDIO
  sudo usermod -a -G audio vagrant
  sudo apt-get install -y oss4-base oss4-dkms oss4-source oss4-gtk linux-headers-3.2.0-23 debconf-utils
  sudo ln -s /usr/src/linux-headers-$(uname -r)/ /lib/modules/$(uname -r)/source || echo ALREADY SYMLINKED
  sudo module-assistant prepare
  sudo module-assistant auto-install -i oss4 # this can take 2 minutes
  sudo debconf-set-selections <<< "linux-sound-base linux-sound-base/sound_system select  OSS"
  echo READY.

  # ---- GIT
  sudo apt-get install -y git

  # ---- sms-tools
  sudo -u vagrant git clone https://github.com/MTG/sms-tools.git

  # ---- sms tools python dependencies
  sudo apt-get install -y python-dev ipython python-numpy python-matplotlib python-scipy python-pygame cython

  # have to reboot for drivers to kick in, but only the first time of course
  if [ ! -f ~/.runonce ]
  then
    sudo reboot
    touch ~/.runonce
  fi
EOF

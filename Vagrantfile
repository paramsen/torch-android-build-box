# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  #===== Constants
  NDK_URL="https://dl.google.com/android/repository/android-ndk-r14b-linux-x86_64.zip"
  NDK_VERSION=24
  NDK_FOLDER="android-ndk-r14b"
  #===== Constants

  config.vm.box = "ubuntu/trusty64"
  config.vm.synced_folder "mounted", "/mounted"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update; apt-get install -y git unzip libx32gcc-4.8-dev libc6-dev-i386
    #Java8
    sudo apt-get install -y python-software-properties debconf-utils
    sudo add-apt-repository -y ppa:webupd8team/java
    sudo apt-get update
    echo "oracle-java8-installer shared/accepted-oracle-license-v1-1 select true" | sudo debconf-set-selections
    sudo apt-get install -y oracle-java8-installer
    #cmake 3.*
    add-apt-repository ppa:george-edison55/cmake-3.x
    apt-get update
    apt-get install -y cmake
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    wget -O ndk.zip #{NDK_URL}
    unzip ndk.zip
    echo "ndkroot=/home/vagrant/#{NDK_FOLDER}" >> ~/.profile
    echo "PATH=/home/vagrant/#{NDK_FOLDER}:$PATH" >> ~/.profile
    source ~/.profile

    #Build Torch: http://torch.ch/docs/getting-started.html#installing-torch
    git clone https://github.com/torch/distro.git ~/torch --recursive
    pushd ~/torch;
    bash install-deps; ./install.sh
    echo ". /home/vagrant/torch/install/bin/torch-activate" >> ~/.profile
    source ~/.profile
    popd

    #Build torch-android: https://github.com/soumith/torch-android#building-torch-libraries-and-java-class
    git clone https://github.com/soumith/torch-android.git ~/torch-android
    pushd ~/torch-android
    git submodule update --init --recursive

    #Conf CUDA=OFF and set correct NDK version
    sed -ie "s/NDKABI=21/NDKABI=#{NDK_VERSION}/" build.sh
    sed -ie 's/WITH_CUDA=${WITH_CUDA:-"ON"}/WITH_CUDA=${WITH_CUDA:-"OFF"}/' build.sh

    #Build for arm-v7n (is default in build.sh)
    ./build.sh
    cp -r install/include/* /mounted/headers
    cp -r install/arm-v7n /mounted
  
    #Build for arm-v8
    sed -ie "s/v7n/v8/" build.sh
    ./clean.sh
    ./build.sh
    cp -r install/arm-v8 /mounted
  SHELL
end

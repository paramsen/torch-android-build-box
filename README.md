# Automated buildbox for torch-android

Run one command and have torch-android for armv7a and armv8 ready for action. 

This Vagrant Box provisions an Ubuntu 14.04 64bit VM with Android SDK/NDK, correct cmake version and other dependencies. After provisioning it builds Torch and torch-android. Finally you can find the built binaries, headers and lua-lib in `./mounted`. Depending on available computation power the build time will be between 15min-2h.

You can use this to build `torch-android` on all platforms supported by VirtualBox; Windows, OSX, Linux etc.

## Instructions

*Prerequisite; Install Vagrant and VirtualBox.*

Let's get started:

1. `git clone https://github.com/paramsen/torch-android-build-box.git`
2. `cd torch-android-build-box`
    * *Protip;* `vim Vagrantfile`, increase RAM/CPUs as much as your computer can afford
    * *Protip 2;* `vim Vagrantfile`, update to the newest NDK if it isn't up to date
3. `mkdir mounted`
4. `vagrant up` -> Vagrant builds torch-android (expect it to take 30min-2hrs depending on the available computation power)
5. `ls mounted` -> Verify that the build completed successfully, `mounted` will contain the following directories each containing built files
    ```
    mounted
    ├── arm64-v8a
    ├── armeabi-v7a
    ├── headers
    └── lua
    ```
6. `vagrant halt` -> Stop the VM
7. `vagrant destroy -f` -> Delete the VM from disk, keeps the built stuff in `./mounted`
8. You're ready to proceed to the next article and use the built binaries in your Android Studio project

## Config (speed up build time)

In the Vagrantfile you can edit available RAM and CPUs, increase these as much as your computer can afford.

## More info about using torch-android

[Read my torch-android writeup for more info](https://paramsen.github.io/torch-android-vagrant-build-box). 

## LICENSE

I created this purely for educational purposes, hence the MIT license.

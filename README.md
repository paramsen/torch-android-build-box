# Vagrant Build Box for torch-android

[Read my torch-android guide for more info.](https://paramsen.github.io/torch-android-vagrant-build-box). 

This Vagrant Box provisions an Ubuntu 14.04 64bit VM with Android SDK/NDK, correct cmake version and other dependencies. After provisioning it builds Torch and torch-android. Finally you can find the built binaries, headers and lua-lib in `./mounted`. Depending on available computation power the build time will be between 15min-2h.

## Config (speed up build time)

In the Vagrantfile you can edit available RAM and CPUs, increase these as much as your computer can afford.

## CUDA

[Read my article on building torch-android with CUDA. (to be written..)](https://paramsen.github.io/torch-android-vagrant-build-box). 

## LICENSE

I created this purely for educational purposes, hence the MIT license.

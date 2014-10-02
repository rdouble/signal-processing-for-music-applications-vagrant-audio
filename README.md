signal-processing-for-music-applications-vagrant-audio
=============
A Vagrant file that creates an audio capable Ubuntu Precice 32 VM on OSX.

This is for the coursera course "Audio Signal Processing for Music Applications"

https://class.coursera.org/audio-001

Uses the OSS sound driver system.

Also installs git, pulls down the sms-tools repo, and additional python libraries.


- install VirtualBox: https://www.virtualbox.org
- install Vagrant: https://www.vagrantup.com
- install X http://xquartz.macosforge.org/landing/
- clone this repo
- Do "vagrant up" to start everthing, then "vagrant ssh" to login to the VM.
- Do "osstest" and you should hear sound.

The ```~/sms-tools/workspace``` directory is a synced folder so you can download programming assignments to the project root, edit them in your favorite text editor on the host OS, and run them on Ubuntu.

based on
https://github.com/GeoffreyPlitt/vagrant-audio
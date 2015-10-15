# Daily cheatsheet

## tmux
+ [tmux configuration](https://gist.github.com/snuggs/800936)
+ `C-s q` : kill panel
+ `C-s |` : create a vertical panel
+ `C-s -` : create a horizontal panel

## Docker
### Installation
``` bash
brew update
brew tap phinze/homebrew-cask
brew install brew-cask
brew cask install virtualbox
brew cask install docker-machine
brew install docker
```
### start daemon
- start by launch: *Docker Quickstart Terminal*
- create a test virtualbox `docker-machine create --driver virtualbox default`
- docker-machine ls/rm/ssh
### run machine
- Run machine: `docker run -i -t gcc /bin/bash`
- [Copying files from host to docker container](http://stackoverflow.com/questions/22907231/copying-files-from-host-to-docker-container)

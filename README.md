# install-spinnaker
Install spinnaker in Ubuntu-16.04 as local environment using halyard
Machine : DigitalOcean 

### 1. Install Halyard
Prerequsites :
  
  a. create an user  `useradd spinnaker`
  
  b. adduser to sudo froup `usermod -aG sudo spinnaker` and login with user `su - spinnaker`
1. Get the packahge `curl -O https://raw.githubusercontent.com/spinnaker/halyard/master/install/debian/InstallHalyard.sh
sudo bash InstallHalyard.sh`
2. check halyard verison `sudo hal -v`
### 2. Emvironrment setup
1. config the env setup as local `sudo hal config deploy edit --type localdebian`
2. choose the version `sudo hal version list` and  `sudo hal config version --version 1.6.1`
### 3. Choose storage as redis
1. `sudo hal config storage edit --type redis`
### 4. deploy the spinnaker
1. `sudo hal deploy apply`

### Expose UI to outside
https://blog.spinnaker.io/exposing-spinnaker-to-end-users-4808bc936698
1. `sudo echo "host: 0.0.0.0" | tee \
    ~/.hal/default/service-settings/gate.yml \
    ~/.hal/default/service-settings/deck.yml`
    
`sudo hal deploy apply`

Note: i expose my Spinnaker outside. While I enabled Gate and Deck to listen on 0.0.0.0, I did not associate a domain name with them which caused things not to work. 
After associating them with a domain name, I was able to make a new application.

`sudo hal config security ui edit \
    --override-base-url http://spinnaker.mydomain.org:9000`

`sudo hal config security api edit \
    --override-base-url http://spinnaker.mydomain.org:8084`
    
`sudo hal deploy apply`

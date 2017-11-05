Install
--------------------------------------------------------------------------------
Download Hasbian from https://github.com/home-assistant
Use Etcher to flash it

https://home-assistant.io/docs/installation/hassbian/installation/


In /home/homeassistant/.homeassistant
----------------------------------------
git init
git remote add origin https://github.com/leifmariposa/Home-AssistantConfig.git
git fetch
git reset origin/master  # this is required if files in the non-empty directory are in the repo
git checkout -t origin/master







Upgrading
--------------------------------------------------------------------------------

https://home-assistant.io/docs/installation/hassbian/upgrading/

Upgrading Hassbian

HASSbian is based on Raspbian and uses the same repositories. Any changes to Raspbian will be reflected in HASSbian. To update and upgrade system packages and installed software (excluding Home Assistant) do the following. Log in as the pi account and execute the following commands:

$ sudo apt-get update
$ sudo apt-get -y upgrade
Updating Home Assistant

You can also use hassbian-config to automate the process by running sudo hassbian-config upgrade home-assistant

To update the Home Assistant installation execute the following command as the pi user.

$ sudo systemctl stop home-assistant@homeassistant.service
$ sudo su -s /bin/bash homeassistant
$ source /srv/homeassistant/bin/activate
$ pip3 install --upgrade homeassistant
$ exit
$ sudo systemctl start home-assistant@homeassistant.service
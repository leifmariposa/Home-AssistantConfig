Last updated on 171105 (Home Assitant v0.57.1)

Installation - https://home-assistant.io/docs/installation/hassbian/installation/
--------------------------------------------------------------------------------
Download Hasbian from https://github.com/home-assistant
Use Etcher to flash it
Boot and wait...

In /home/homeassistant/.homeassistant
----------------------------------------
git init
git remote add origin https://github.com/leifmariposa/Home-AssistantConfig.git
git fetch
git reset origin/master  # this is required if files in the non-empty directory are in the repo
git checkout -t origin/master




Upgrading - https://home-assistant.io/docs/installation/hassbian/upgrading/
--------------------------------------------------------------------------------
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



Glances
--------------------------------------------------------------------------------
sudo apt-get install glances

sudo nano /etc/systemd/system/glances.service
[Unit]
Description=Glances

[Service]
ExecStart=/usr/bin/glances -w
Restart=on-abort

[Install]
WantedBy=multi-user.target


sudo systemctl enable glances.service
sudo systemctl start glances.service


http://192.168.2.108:61208/

sensors.yaml

- platform: glances
  host: 127.0.0.1
  resources:
    - 'disk_use_percent'
    - 'disk_use'
    - 'disk_free'
    - 'memory_use_percent'
    - 'memory_use'
    - 'memory_free'
    - 'swap_use_percent'
    - 'swap_use'
    - 'swap_free'
    - 'processor_load'
    - 'process_running'
    - 'process_total'
    - 'process_thread'
    - 'process_sleeping'
    - 'cpu_temp'
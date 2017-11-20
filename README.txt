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



Remote Access with TLS/SSL via Let's Encrypt
--------------------------------------------------------------------------------
https://home-assistant.io/docs/ecosystem/certificates/lets_encrypt/

1. SET UP PORT FORWARDING WITHOUT TLS/SSL AND TEST CONNECTION
   Service name - ha_test
   Port Range - 8123
   Local IP - YOUR-HA-IP
   Local Port - 8123
   Protocol - Both

2. Test
   http://<My IP>:8123

3. Test with a DNS service
   http://leifmariposa.asuscomm.com:8123

4. OBTAIN A TLS/SSL CERTIFICATE FROM LET’S ENCRYPT
   First set up port forwardning for http

   Service name - ha_letsencrypt
   Port Range - 80
   Local IP - YOUR-HA-IP
   Local Port - 80
   Protocol - Both

5. sudo su -s /bin/bash hass
   cd
   mkdir certbot
   cd certbot/
   wget https://dl.eff.org/certbot-auto
   chmod a+x certbot-auto
   exit

   <as pi user>
   cd /home/homeassistant/certbot
   ./certbot-auto certonly --standalone --preferred-challenges http-01 --email your@email.address -d examplehome.duckdns.org
   ./certbot-auto certonly --standalone --preferred-challenges http-01 --email leifmariposa@hotmail.com -d leifmariposa.asuscomm.com

   sudo chmod 755 /etc/letsencrypt/live/
   sudo chmod 755 /etc/letsencrypt/archive/

6. CHECK THE INCOMING CONNECTION
   Service name - ha_ssl
   Port Range - 443
   Local IP - YOUR-HA-IP
   Local Port - 8123
   Protocol - Both

7. configuration.yaml
http:
  api_password: !secret api_password
  ssl_certificate: /etc/letsencrypt/live/leifmariposa.asuscomm.com/fullchain.pem
  ssl_key: /etc/letsencrypt/live/leifmariposa.asuscomm.com/privkey.pem
  base_url: leifmariposa.asuscomm.com

8. Test
   https://leifmariposa.asuscomm.com

9. CLEAN UP PORT FORWARDS
   Service name - ha_test

10. SET UP A SENSOR TO MONITOR THE EXPIRY DATE OF THE CERTIFICATE
    sudo apt-get update
    sudo apt-get install ssl-cert-check

<sensor.yaml>
- platform: command_line
  name: SSL cert expiry
  unit_of_measurement: days
  scan_interval: 10800
  command: "ssl-cert-check -b -c /etc/letsencrypt/live/leifmariposa.asuscomm.com/cert.pem | awk '{ print $NF }'"

<groups.yaml>
misc_view:
  view: yes
  icon: mdi:all-inclusive
  entities:
    - group.misc

misc:
  name: SSL Cert Expiry
  entities:
    - sensor.ssl_cert_expiry

11. SET UP AN AUTOMATIC RENEWAL OF THE TLS/SSL CERTIFICATE
<configuration.yaml>
shell_command:
  renew_ssl: ~/certbot/certbot-auto renew --quiet --no-self-upgrade --standalone --preferred-challenges http-01

<automations.yaml>
- alias: 'Auto Renew SSL Cert'
  trigger:
    platform: numeric_state
    entity_id: sensor.ssl_cert_expiry
    below: 29
  action:
    service: shell_command.renew_ssl





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
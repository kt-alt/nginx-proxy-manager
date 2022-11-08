# Nginx Proxy Manager

This project provides the artifacts needed to stand up a container with nginx and the proxy manager app installed.  It's based on the steps outlined here:

https://nginxproxymanager.com/guide/#quick-setup
- default login is admin@example.com / changeme.

https://www.youtube.com/watch?v=P3imFC7GSr0
- nginx proxy manager install & config

To configure nginx as a reverse proxy for wordpress, pi-hole, and any other applications, there are 2 broad steps (+ optional ssl):

1.  Create local DNS entries, for example mywordpress.home.arpa, and map these to the ip address of nginx so that any requests will be routed first the nginx proxy.  This can be done in the pi-hole admin UI.  Otherwise pi-hole's configuration file for custom local dns entries is located in the container's /etc/pi-hole/custom.list.

2.  Create proxy host entries which map the incoming source hostname to the underlying app's server/container's ip:port.  Nginx will pass the request through to the underlying app and the pass back the response to the client.  Note if you want to use the short name (ie mywordpress), you will need to define mywordpress as an alternate source domain for each of the desired proxy hosts (in addition to the FQDN mywordpress.home.arpa).

3.  Since there is a reverse proxy, this is where you should centrally configure & manage https/ssl for all of the underlying apps.  To do this upload the server cert & private key for each of the desired hostnames.  Then enable SSL for each of the desired proxy hosts.

## Extra Step for Wordpress

You will need to configure the Wordpress & Site Address (in wordpress's General Setttings) to use the FQDN mywordpress.home.arpa.  If you use the short name mywordpress, you'll get the REST API / loopback request errors in wordpress's Site Health area and this will prevent things like the scheduled maintenance tasks (like backups) from firing.

## Extra Step for Pihole

If you want users to be able to just point to http://mypilehole_dnsname (no /admin), you will need to define a Custom Location in the Proxy Host as follows:
- location:  /
- scheme:  http
- forward hostname/ip:  pihole_ip
- forward port: pihole_port
- "gear button":  rewrite ^/$ /admin break;


## Manual Configuration of Nginx as a Reverse Proxy

Better to use the nginx-proxy-manager as it's all UI driven and faster, but if you want/need to manually configure nginx as a reverse proxy you can follow some of the tutorials below:

https://www.youtube.com/watch?v=T7flzYEtMGA
- set up https for worpress with nginx

https://www.youtube.com/watch?v=wQcSql62zRo
- how to add ssl to web apps using nginx reverse proxy

https://www.youtube.com/watch?v=QcnAqN_Ihqk
- how to set up (nginx) reverse proxy on home network


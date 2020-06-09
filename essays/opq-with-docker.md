---
layout: essay
type: essay
title: Bringing up OPQ using Docker
# All dates must be YYYY-MM-DD format!
date: 2020-06-19
labels:
  - Software Engineering
  - Docker
  - Concepts
---


   Bringing up OPQ was not too hard, but I did run into some problems. The instructions were pretty easy to follow, thought I was a bit lost as to what I was doing when I was doing them, particularly as to what the different services do (Mauka, Makai, etc.) and how they interact with each other.

   We already got Docker up and running so we breezed through that part. Then it was a matter of simply following directions, going into certain folders, running certain files, not too bad. 

   I did run into some memory management problems on my end. Calvan did not experience this. I believe running certain files had maxed out my local memory and my computer was telling me I did not have a single byte to spare. Not sure how true that is. Docker also kept crashing and it took many attempts just to get to keep running. I am not sure if my memory issue had something to do with this.
After downloading everything and verifying everything is running, we proceed to the HTTPS section. Besides running into a setback with getting a domain name, I also had some trouble getting the SSH certificate working. Ill still try to fiddle with it and see where it takes me, but this was the correct output, and following that is what I obtained.


`opquser@emilia:~/opq-docker$ ./init-letsencrypt.sh
Existing data found for emilia.ics.hawaii.edu. Continue and replace existing certificate? (y/N) y
### Creating dummy certificate for emilia.ics.hawaii.edu ...
WARNING: The NGINX_SERVER_NAME variable is not set. Defaulting to a blank string.
Creating network "opq-docker_default" with the default driver
Creating opq-docker_boxupdateserver_1 ... done
Creating opq-mongo                    ... done
Creating opq-docker_view_1            ... done
Creating opq-docker_nginx_1           ... done
Generating a RSA private key
........................+++++
............+++++
writing new private key to '/etc/letsencrypt/live/emilia.ics.hawaii.edu/privkey.pem'
-----

### Starting nginx ...
WARNING: The NGINX_SERVER_NAME variable is not set. Defaulting to a blank string.
Stopping opq-docker_view_1            ... done
Stopping opq-mongo                    ... done
Stopping opq-docker_boxupdateserver_1 ... done
Removing opq-docker_nginx_1           ... done
Removing opq-docker_view_1            ... done
Removing opq-mongo                    ... done
Removing opq-docker_boxupdateserver_1 ... done
Removing network opq-docker_default
Creating network "opq-docker_default" with the default driver
Creating opq-mongo                    ... done
Creating opq-docker_boxupdateserver_1 ... done
Creating opq-docker_view_1            ... done
Creating opq-docker_makai_1           ... done
Creating opq-docker_nginx_1           ... done
Creating opq-docker_mauka_1           ... done
Creating opq-docker_certbot_1         ... done
Creating opq-docker_health_1          ... done

### Deleting dummy certificate for emilia.ics.hawaii.edu ...
Starting opq-docker_boxupdateserver_1 ... done
Starting opq-mongo                    ... done
Starting opq-docker_view_1            ... done
Starting opq-docker_nginx_1           ... done

### Requesting Let's Encrypt certificate for emilia.ics.hawaii.edu ...
Starting opq-docker_boxupdateserver_1 ... done
Starting opq-mongo                    ... done
Starting opq-docker_view_1            ... done
Starting opq-docker_nginx_1           ... done
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator webroot, Installer None

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for emilia.ics.hawaii.edu
Using the webroot path /var/www/certbot for all unmatched domains.
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/emilia.ics.hawaii.edu/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/emilia.ics.hawaii.edu/privkey.pem
   Your cert will expire on 2019-06-29. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"

### Reloading nginx ...
2019/03/31 21:58:34 [notice] 10#10: signal process started`












`arslanr@Arslans-Laptop:~/opq-docker$ ./init-letsencrypt.sh
Existing data found for arslanr.com. Continue and replace existing certificate? (y/N) y
Deleting existing files in ./data/certbot. Sudo is used in case the ./data/certbot files are owned by root, which can occur if Docker Compose has already been previously spun-up prior to running this script.
[sudo] password for arslanr:
./init-letsencrypt.sh: line 30: sestatus: command not found
./init-letsencrypt.sh: line 30: [: ==: unary operator expected
### Downloading recommended TLS parameters ...

### Creating dummy certificate for arslanr.com ...
WARNING: The NGINX_SERVER_NAME variable is not set. Defaulting to a blank string.
Starting opq-docker_boxupdateserver_1 ... done
Starting opq-mongo                    ... done
Starting opq-docker_view_1            ... done
Starting opq-docker_nginx_1           ... error

ERROR: for opq-docker_nginx_1  Cannot start service nginx: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"rootfs_linux.go:58: mounting \\\"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Ubuntu/9121ff1d71a1486bd595b1107133c5506fb558fc8f10b51758b41b27276a82c5\\\" to rootfs \\\"/var/lib/docker/overlay2/d7decbc4594a6819804945e934ee97f5ca651b178e2b9544dad3b7f629d2d5fa/merged\\\" at \\\"/var/lib/docker/overlay2/d7decbc4594a6819804945e934ee97f5ca651b178e2b9544dad3b7f629d2d5fa/merged/etc/letsencrypt\\\" caused \\\"no such file or directory\\\"\"": unknown

ERROR: for nginx  Cannot start service nginx: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"rootfs_linux.go:58: mounting \\\"/run/desktop/mnt/host/wsl/docker-desktop-bind-mounts/Ubuntu/9121ff1d71a1486bd595b1107133c5506fb558fc8f10b51758b41b27276a82c5\\\" to rootfs \\\"/var/lib/docker/overlay2/d7decbc4594a6819804945e934ee97f5ca651b178e2b9544dad3b7f629d2d5fa/merged\\\" at \\\"/var/lib/docker/overlay2/d7decbc4594a6819804945e934ee97f5ca651b178e2b9544dad3b7f629d2d5fa/merged/etc/letsencrypt\\\" caused \\\"no such file or directory\\\"\"": unknown
ERROR: Encountered errors while bringing up the project.

### Starting nginx ...
Stopping opq-docker_view_1            ... done
Stopping opq-docker_certbot_1         ... done
Stopping opq-docker_makai_1           ... done
Stopping opq-mongo                    ... done
Removing opq-docker_certbot_1         ... done
Removing opq-docker_health_1          ... done
Removing opq-docker_nginx_1           ... done
Removing opq-docker_mauka_1           ... done
Removing opq-docker_view_1            ... done
Removing opq-docker_boxupdateserver_1 ... done
Removing opq-docker_makai_1           ... done
Removing opq-mongo                    ... done
Removing network opq-docker_default
Creating network "opq-docker_default" with the default driver
Creating opq-docker_boxupdateserver_1 ... done
Creating opq-mongo                    ... done
Creating opq-docker_makai_1           ... done
Creating opq-docker_view_1            ... done
Creating opq-docker_nginx_1           ... done
Creating opq-docker_mauka_1           ... done
Creating opq-docker_certbot_1         ... done
Creating opq-docker_health_1          ... done

### Deleting dummy certificate for arslanr.com ...
Starting opq-docker_boxupdateserver_1 ... done
Starting opq-mongo                    ... done
Starting opq-docker_view_1            ... done
Starting opq-docker_nginx_1           ... done

### Requesting Let's Encrypt certificate for arslanr.com ...
Starting opq-docker_boxupdateserver_1 ... done
Starting opq-mongo                    ... done
Starting opq-docker_view_1            ... done
Starting opq-docker_nginx_1           ... done
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator webroot, Installer None

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for arslanr.com
Using the webroot path /var/www/certbot for all unmatched domains.
Waiting for verification...
Challenge failed for domain arslanr.com
http-01 challenge for arslanr.com
Cleaning up challenges
Some challenges have failed.

IMPORTANT NOTES:
 - The following errors were reported by the server:

   Domain: arslanr.com
   Type:   connection
   Detail: Fetching
   http://www.arslanr.com/.well-known/acme-challenge/q2lOlQ9nyXcTHXxtp_vNh8rnZctcHr7bKnkqDH2Le-0:
   Connection reset by peer

   To fix these errors, please make sure that your domain name was
   entered correctly and the DNS A/AAAA record(s) for that domain
   contain(s) the right IP address. Additionally, please check that
   your computer has a publicly routable IP address and that no
   firewalls are preventing the server from communicating with the
   client. If you're using the webroot plugin, you should also verify
   that you are serving files from the webroot path you provided.
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - We were unable to subscribe you the EFF mailing list because your
   e-mail address appears to be invalid. You can try again later by
   visiting https://act.eff.org.

### Reloading nginx ...
ERROR: No container found for nginx_1
arslanr@Arslans-Laptop:~/opq-docker$`

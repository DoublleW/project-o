# Start playbook on hosts which need to configured on kubernetes

 - Runs on `Ubuntu 18.04.4 LTS` / `Debian GNU/Linux 9 (stretch)`

 - Primary host configuration.

 - Deletes the old configuration and cluster.

 - Install and configuration on a separate host (the border host accepts incoming connections on ports `80/443`)


 - File `group_vars/all_servers.yml` contains user settings with `sudo` privileges, profile path key `id_rsa.pub`, `hosts`, kubernetes user, time zone (`$ timedatectl list-timezones`).
 - File `hosts.txt` contains host IP addresses

Before starting

 -- comment out the unnecessary tasks in the `deploy.yml` file.
 
 -- correct ip addresses of hosts in `hosts.txt` file.

 -- check the variables in `group_vars/all_servers.yml` file.

 - Run command 
```
$ ansible-playbook deploy.yml -K
```


# Enter at master host

 - Enter at master host as user `kubemaster` (`kube_user` in `group_vars/all_servers.yml`)


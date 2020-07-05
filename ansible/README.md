# Start playbook on hosts which need to configured on kubernetes

 - Runs on `Ubuntu 18.04.4 LTS` / `Debian GNU/Linux 9 (stretch)`

 - Primary host configuration.

 - Deletes the old configuration and cluster.

 - Install and configuration on a separate host (the border host accepts incoming connections on ports `80/443`)


 - File `group_vars/all_servers.yml` contains user settings with `sudo` privileges, profile path key `id_rsa.pub`, `main_host`, kubernetes user, ports, time zone (`$ timedatectl list-timezones`).
 - File `hosts.ini` contains host IP addresses.

Before starting

comment out the unnecessary tasks in the `deploy.yml` file.
 
correct ip addresses of hosts in `hosts.ini` file.

check the variables in `group_vars/all_servers.yml` file.

 - Run command 
```
$ ansible-playbook deploy.yml

## or

$ ansible-playbook deploy.yml -K
```

---

# Enter at `master` host

 - Enter at master host as user `kubemaster` (`kube_user` in `group_vars/all_servers.yml`)

---

# Install `nginx`

 - Installing `nginx` on an edge host with forwarding to kubernetes workers on internal `80/tcp` port
 - All configuration files are created in the `/opt/dev-ops` folder. A soft link is created in the `nginx` folder to the files in the `/opt/dev-ops...` folder

 - How to check, in browser enter `http://<nginx_ip>:<port>/nginx_status`

when:

`<nginx_ip>` in `hosts.ini` - `[hosts_o]`
    
`<port>` in `group_vars/all_servers.yml` - `stub_status_port`

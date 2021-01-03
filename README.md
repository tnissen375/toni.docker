toni.docker
=========
# Role has to be refactored. Its quick & dirty state.

Ansible role to install docker, create networks, install node-exporter.
Its possible to run docker with compose, as systemd service (example included in openresty role) or as stack in swarm mode.

Only basic swarm setup is done by the required role - by now. (to be adopted)

Requirements
------------

roles:
  - name: atosatto.docker-swarm

Role Variables
--------------

```bash
user_path: "/root"
docker_setup_networks: 'true'  # create networks, configuration in vars file or role defaults
docker_network_driver: 'bridge' # bridge (local: docker-compose) or overlay (swarm: stack)
docker_network_encrypted: 'false' 
users: [] # an array of users. These will be added to the docker system group 
    # docker_networks: By default 2 local networks are created - nginx_net and stack_com
    # Have a look at defaults to get the clue, configure to your needs in vars file or in defaults

# All other values are only necessary if monitoring gets enabled
configure_monitoring: false  
node_exporter_ip: '127.0.0.1' # Default listen ip of node_exporter, may be changed to wireguard ip if used together
node_exporter_port: 9100 # Default listen port of node_exporter
# Always check if there is a newer stable release:
# https://hub.docker.com/r/prom/node-exporter/tags
node_exporter_image: "prom/node-exporter:v1.0.1"
```


Example Playbook
----------------
```yaml
- hosts: swarm-01
  remote_user: root
  gather_facts: yes
  roles:
    - {
      role: docker,
      docker_setup_networks: true,
      docker_network_encrypted: true
      }
```

License
-------

MIT

Author Information
------------------

T.Nissen



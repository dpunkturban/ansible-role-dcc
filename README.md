# Ansible Role: DCC (Distributed Checksum Clearinghouse)

This Ansible role can be used to install the client and/or server of the DCC. More information about DCC can be found at https://www.dcc-servers.net/dcc/.

## ToDo:
- [ ] make whiteclnt more flexible

## Tags:

* `install` - Should only be used for installation


* `configure` - Should be used for configuration changes

## Variables:

* `role_dcc_greylist_ids`: `2.3.167` - Version to be installed. You can find the latest version on the website https://www.dcc-servers.net/dcc/ or on https://www.dcc-servers.net/src/dcc/old/

* `role_dcc_reinstall`: `no` - Force reinstallation proccess

* `role_dcc_user`: `dcc` - User under which the program should run

* `role_dcc_group`: `dcc` - Group under which the program should run

* `role_dcc_bind_ip`: `127.0.0.1` - Binded IP-Adress

* `role_dcc_client_port`: `"11344"` - Port for external access, e.g. rSpamd

* `role_dcc_net`: `"0.0.0.0/0"` - Allowed networks

* `role_dcc_config`: `""` - DCC-Config for Client and/or Server. It is possible to run server and client simultaneously. For this, the configuration of server and client must be stored in the variable and the host must be in both Ansible groups. By default, only the client configuration is installed.

Example:

Client config:
```yaml
  - name: DCCIFD_ENABLE
    line: 'DCCIFD_ENABLE=on'
  - name: DCCUID
    line: 'DCCUID=dcc'
  - name: DCCIFD_LOG_AT
    line: '#DCCIFD_LOG_AT=NEVER'
  - name: DBCLEAN_LOGDAYS
    line: 'DBCLEAN_LOGDAYS=1'
  - name: DCCIFD_LOGDIR
    line: '#DCCIFD_LOGDIR=""'
  - name: DCCIFD_ARGS
    line: 'DCCIFD_ARGS="-SHELO -Smail_host -SSender -SList-ID -p {{ role_dcc_bind_ip }},{{ role_dcc_client_port }},{{ role_dcc_net }}"'
```

Server config (**-> Ensure that you have a server ID <-**):
```yaml

  - name: DCCD_ENABLE
    line: 'DCCD_ENABLE=on'
  - name: SRVR_ID
    line: 'SRVR_ID={{ role_dcc_server_id }}'
  - name: BRAND
    line: 'BRAND={{ role_dcc_server_brand }}'
  - name: DCCD_ARGS
    line: 'DCCD_ARGS="-i {{ role_dcc_server_id }} -n {{ role_dcc_server_brand }} -a 127.0.0.1 -a {{ ansible_default_ipv4.address }} -u FOREVER"'
  - name: REP_ARGS
    line: 'REP_ARGS='
```
* `role_dcc_map_ipv6`: `yes` - Enable/Disable IPv6

### Client Config

* `role_dcc_map_server`:  - Servers to be queried. By default, all public servers are requested. Public servers should only be used if less than 100,000 emails are processed per day. Please see the installation instructions for more information. If no public servers are used, a secret must also be added.

Example:

Public Server:
```yaml
  - hostname: dcc1.dcc-servers.net
    options: "RTT+1000 ms"
    clientid: anon
    secret:
  - hostname: dcc2.dcc-servers.net
    options: "RTT+1000 ms"
    clientid: anon
    secret:
  - hostname: dcc3.dcc-servers.net
    options: "RTT+1000 ms"
    clientid: anon
    secret:
  - hostname: dcc4.dcc-servers.net
    options: "RTT+1000 ms"
    clientid: anon
    secret:
  - hostname: dcc5.dcc-servers.net
    options: "RTT+1000 ms"
    clientid: anon
    secret:
```

### Server Config

* `role_dcc_server_id`: `0000` - Unique server-ID. Steps to obtain a server-ID are described in the installation guide (see https://www.dcc-servers.net/dcc/INSTALL.html)

* `role_dcc_server_brand`: `Example` - BRAND can be any short alphanumeric string that suggests the identity of the server.

* `role_dcc_client_ids`:  - Management of client IDs

* `role_dcc_server_ids`:  - Management of server IDs

* `role_dcc_server_flod_peers`:  - Management of your flod peers. Flooding requires that every server participating in a network of DCC servers have a unique server-ID (see https://www.dcc-servers.net/dcc/INSTALL.html).


## License
Eupl-1.2 license


## Author Information
This role:  was created by: dpunkturban (Dennis Urban)

Documentation generated using: [Ansible-autodoc](https://github.com/AndresBott/ansible-autodoc)


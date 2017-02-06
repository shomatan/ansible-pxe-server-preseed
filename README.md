# Ansible role: pxe-server-preseed
Installs and configures PXE/TFTP server.

## Requirements
None.

## Role Variables
|Key|Type|Description|Default|
|:--|:---|:----------|:------|
|pxe_server_preseed_tftpboot_path|String||/var/lib/tftpboot/debian|
|pxe_server_preseed_file_path|String||/var/www/html/preseed|
|pxe_server_preseed_listen|String||127.0.0.1:8188|
|pxe_server_preseed_iso_file_path|String||{{ pxe_server_preseed_tftpboot_path }}/ubuntu-16.04.iso|
|pxe_server_preseed_iso_mount_path|String||/var/www/html/ubuntu-16.04|
|pxe_server_preseed_iso_download_path|String||[ubuntu-16.04.1-server-amd64.iso](http://ftp.riken.jp/Linux/ubuntu-releases/16.04.1/ubuntu-16.04.1-server-amd64.iso)|
|pxe_server_preseed_default_timeout|Integer||300|
|pxe_server_preseed_default_title|String||Debian PXE Boot Menu|
|pxe_server_preseed_default_menu|Hash|Key: display label name. Value: kickstart file name.|{}|

## Dependencies
- [dhcpd](https://github.com/shomatan/ansible-dhcpd.git)
- [nginx](https://github.com/shomatan/ansible-nginx.git)
- [tftp](https://github.com/shomatan/ansible-tftp.git)
- [xinetd](https://github.com/shomatan/ansible-xinetd.git)

## Example playbook

```yaml
- hosts: all
  roles:
    - role: pxe-server-kickstart-preseed
  vars:
    pxe_server_kickstart_listen: "192.168.33.2:8188"
    pxe_server_kickstart_default_menu:
      "Install Ubuntu 16.04": "default.cfg"
    dhcpd_bind_interface: enp0s8
    dhcpd_subnets:
      - subnet: 192.168.33.0
        routers: 192.168.33.5
        netmask: 255.255.255.0
        next_server: 192.168.33.2
        range: "192.168.33.240 192.168.33.250"
        options:
          filename: debian/pxelinux.0
```

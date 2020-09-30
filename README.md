# ansible-pihole

This role will install the ad blocker Pi-hole on the node. The role skeleton and test code is based on [hannseman/ansible-raspbian](https://github.com/hannseman/ansible-raspbian) - thanks and kudos to [Hannes Ljungberg](https://github.com/hannseman).

### It will:

 * Determine if Pi-hole is already installed (by calling `pihole -v`)
 * Install Pi-hole if not present

### It will not:

 * Set any network config (should be given or be done in other roles)
 * Reboot the system

## Setup
* Flash SD-card with [Raspbian Stretch  Lite or Buster Lite](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md).
* Add empty file named `ssh` in boot-partition of the flashed SD-card.
* Optional: To enable wifi place a file called `wpa_supplicant.conf` in the boot-partition of the flashed SD-card with the following content:
```
network={
        ssid="your ssid"
        psk="your password"
}
```
* Boot your Raspberry Pi.
* Run playbook.

## Variables

```yaml
---
# Network interface to make Pi-hole listen on
pihole_interface: eth0

# The Pi's IPv4 and IPv6 addresses
pihole_ipv4_address: "{{ hostvars[inventory_hostname]['ansible_' + pihole_interface]['ipv4']['address'] }}/24"
pihole_ipv6_address: "{{ hostvars[inventory_hostname]['ansible_' + pihole_interface]['ipv6'][0]['address'] }}"

# List of DNS servers to use. Each address can be either IPv4 or IPv6
pihole_dns_servers:
  - "192.168.0.1"
  - "192.168.0.2"
```

## Example Playbook
```yaml
- hosts: servers
  become: true
  roles:
    - role: ansible-pihole
  vars:
    pihole_interface: eth0
	pihole_dns_servers:
     - "192.168.0.1"
     - "192.168.0.2"
```

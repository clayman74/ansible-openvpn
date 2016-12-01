Ansible OpenVPN role
=========

[![Build Status](https://travis-ci.org/clayman74/ansible-openvpn.svg?branch=master)](https://travis-ci.org/clayman74/ansible-openvpn)

This role install and configure OpenVPN server and clients.


Requirements
------------

This role requires:
  - ansible 2.2


Role Variables
--------------

    openvpn_key_country: "RU"
    openvpn_key_province: ""
    openvpn_key_city: ""
    openvpn_key_org: ""
    openvpn_key_ou: ""
    openvpn_request_subject: "/C={{ openvpn_key_country }}/ST={{ openvpn_key_province }}/L={{ openvpn_key_city }}/O={{ openvpn_key_org }}/OU={{ openvpn_key_ou }}"

    openvpn_days_valid: "365"
    openvpn_key_size: "2048"
    openvpn_cipher: "AES-256-CBC"
    openvpn_auth_digest: "SHA512"
    openvpn_path: "/etc/openvpn"
    openvpn_ca: "{{ openvpn_path }}/keys/ca"
    openvpn_dhparam: "{{ openvpn_path }}/keys/dh{{ openvpn_key_size }}.pem"
    openvpn_hmac_firewall: "{{ openvpn_path }}/keys/ta.key"

    openvpn_server: ""
    openvpn_proto: "udp"
    openvpn_port: "1194"
    openvpn_dev: "tun"

    openvpn_tls_version_min: "tls-version-min 1.2"
    openvpn_tls_cipher: "tls-cipher TLS-ECDHE-RSA-WITH-AES-128-GCM-SHA256:TLS-ECDHE-ECDSA-WITH-AES-128-GCM-SHA256:TLS-ECDHE-RSA-WITH-AES-256-GCM-SHA384:TLS-DHE-RSA-WITH-AES-256-CBC-SHA256"

    openvpn_clients: []


Example Playbook
----------------

Inventory should be looks like:


    localhost ansible_connection=local

    [master]
    master ansible_host=192.168.1.2 ansible_become=true ansible_user=root

    [slaves]
    slave_1 ansible_host=192.168.1.3 ansible_become=true ansible_user=root
    slave_2 ansible_host=192.168.1.4 ansible_become=true ansible_user=root

    [private_network]
    master
    slave_1
    slave_2

    [private_network:vars]
    openvpn_server='master'
    openvpn_clients=['slave_1', 'slave_2']


Playbook should be looks like:

    - hosts: private_network
      roles:
        - openvpn


License
-------

MIT

Author Information
------------------

Kirill Sumorokov

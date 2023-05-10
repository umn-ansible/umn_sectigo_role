Sectigo-Certbot
=========

This role automates placing the account files for Sectigo on your host, so that certbot can request Sectigo/Incommon certs instead of LetsEncrypt.

Requirements
------------

You'll need to get setup with Sectigo before you can use this role. Email sslcerts@umn.edu to get started.
They'll provide you with credentials - an EAB KID and EAD HMAC. 

In your playbook, include this role before including `geerlingguy.certbot`. Below is an example of the full configuration which uses the shared Apache role, certbot, and Sectigo.

```
sectigo_eab_kid: YOUR-KID
sectigo_eab_hmac: YOUR-HMAC
apache_vhost_ssl_certs_dir: "/etc/letsencrypt/live/{{ hostname }}"
apache_vhost_ssl_keys_dir: "/etc/letsencrypt/live/{{ hostname }}"
apache_deploy_ssl_certs: false
certbot_auto_renew_user: root
certbot_auto_renew: true
certbot_auto_renew_options: "--agree-tos --email=latistecharch@umn.edu --quiet --no-self-upgrade --pre-hook 'service httpd24-httpd stop' --post-hook 'service httpd24-httpd start'"
certbot_admin_email: your-email
certbot_install_from_source: false
certbot_create_if_missing: true
certbot_create_standalone_stop_services: [httpd24-httpd]

certbot_certs:
  - domains:
    - "{{ hostname }}"

apache_vhosts:
- server_name: "{{ hostname }}"
  use_ssl: true
  document_root: "/swadm/var/www/html/current/public"
  ssl_certificate_cer_file: "cert.pem"
  ssl_certificate_interm_cer_file: "chain.pem"
  ssl_certificate_key_file: "privkey.pem"
  
```




Role Variables
--------------

`sectigo_eab_kid:` provided by sslcerts@umn.edu
`sectigo_eab_hmac` provided by sslcerts@umn.edu

Dependencies
------------

`geerlingguy.cerbot`


License
-------

BSD

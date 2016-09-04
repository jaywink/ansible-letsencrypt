[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-letsencrypt-blue.svg?style=flat-square)](https://galaxy.ansible.com/jaywink/letsencrypt)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](https://tldrlegal.com/license/mit-license)

## Ansible LetsEncrypt

A role to automate LetsEncrypt certificates.

Stability: beta.

### What does it do?

This role will pull in the official [Certbot client](https://github.com/certbot/certbot), install it and issue or renew a certificate with your chosen domain.

Currently the role is of limited functionality as follows:
* Tested on Ubuntu 14.04
* Using Apache2 to do validation
* One domain per role include only
* No automatic installation, certonly mode only

PR's are welcome to include more functionality.

#### More detail

* The client will be installed in `/opt/certbot` as root
* Each run will pull in the latest Certbot client code. You can set a specific Certbot version using the variable `letsencrypt_certbot_version`.
* The role will by default stop Apache2 before requesting a cert and then restart it after done. The list of services to be stopped and started can be configured using the variable `letsencrypt_pause_services`.
* `certonly` mode is used, which means no automatic web server installation
* After cert issuing, you can find it in `/etc/letsencrypt/live/<domainname>`
   * Tip, use this in your Apache2 config, for example, in your main role. Just make sure not to try and start Apache2 with the virtualhost active without the LetsEncrypt role running first!

       ```
       SSLCertificateFile /etc/letsencrypt/live/{{ hostname }}/cert.pem
       SSLCertificateKeyFile /etc/letsencrypt/live/{{ hostname }}/privkey.pem
       SSLCertificateChainFile /etc/letsencrypt/live/{{ hostname }}/chain.pem
       ```

* Note! If this role fails in the cert request part, you might have stopped services - take care!
* If the cert has been requested before, this role will automatically try to renew it, if possible. Disable this functionality by setting `letsencrypt_force_renew` to `false`.
* A `www.` subdomain will automatically be requested along with the certificate.
    * To disable this behaviour, set `letsencrypt_request_www` to `false` in your vars.

### Requirements

Tested with the following:

* Ubuntu 14.04
* Apache2
* Ansible 1.9 / 2.x

### Role Variables

#### Required

* `letsencrypt_domain` - Domain the certificate is for.
* `letsencrypt_email` - Your email as certificate owner.

#### Optional

* `letsencrypt_certbot_args` - Additional command line args to Certbot.
* `letsencrypt_certbot_version` - Set specific Certbot version, for example a git tag or branch.
* `letsencrypt_force_renew` - Whether to attempt renewal always, default to `true`.
* `letsencrypt_pause_services` - List of services to stop/start while calling Certbot. Defaults to `apache2`.
* `letsencrypt_request_www` - Request `www.` automatically (default `true`).

### Example Playbook

This role works best when included just before your main site role, for example. Or it can be used in an individual playbook, for example as below.

This role should become root on the target host.

    ---
    - hosts: myhost
      become: yes
      become_user: root
      roles:
        - { role: ansible-letsencrypt, letsencrypt_email: email@example.com, letsencrypt_domain: example.com }

### License

MIT

### Author Information

Jason Robinson - mail@jasonrobinson.me - https://iliketoast.net/u/jaywink - https://twitter.com/jaywink

See CONTRIBUTORS for wonderful people who have expanded the role <3

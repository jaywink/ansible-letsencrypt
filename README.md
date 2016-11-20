[![Build Status](https://travis-ci.org/jaywink/ansible-letsencrypt.svg?branch=master)](https://travis-ci.org/jaywink/ansible-letsencrypt)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-letsencrypt-blue.svg?style=flat-square)](https://galaxy.ansible.com/jaywink/letsencrypt)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](https://tldrlegal.com/license/mit-license)

## Ansible LetsEncrypt

A role to automate LetsEncrypt certificates.

Stability: beta.

Ansible version required: 2.x

### What does it do?

This role will pull in the official [Certbot client](https://github.com/certbot/certbot), install it and issue or renew a certificate with your chosen domain.

Functionality as follows:
* Tested on Ubuntu 14.04 and Debian 8
* One domain per role include only
* Runs in `certonly` mode only


PR's are welcome to include more functionality.

#### More detail

* The client will be installed in `/opt/certbot` as root
* Each run will pull in the Certbot client code from a proven release version. You can set a specific Certbot version using the variable `letsencrypt_certbot_version`.
* A list of services to be stopped before and (re-)started after obtaining a new certificate can be configured using the variable `letsencrypt_pause_services`.
* `certonly` mode is used, which means no automatic web server installation
* After cert issuing, you can find it in `/etc/letsencrypt/live/<domainname>`
   * Tip, use this in your Apache2 config, for example, in your main role. Just make sure not to try and start Apache2 with the virtualhost active without the LetsEncrypt role running first!

       ```
       SSLCertificateFile /etc/letsencrypt/live/{{ letsencrypt_domain }}/cert.pem
       SSLCertificateKeyFile /etc/letsencrypt/live/{{ letsencrypt_domain }}/privkey.pem
       SSLCertificateChainFile /etc/letsencrypt/live/{{ letsencrypt_domain }}/chain.pem
       ```

* Note! If this role fails in the cert request part, you might have stopped services - take care!
* If the cert has been requested before, this role will automatically try to renew it, if possible. Disable this functionality by setting `letsencrypt_force_renew` to `false`. No renewal will be attempted in this case if cert is not due for renewal.
* A `www.` subdomain will automatically be requested along with the certificate.
    * To disable this behaviour, set `letsencrypt_request_www` to `false` in your vars.

### Requirements

Tested with the following:

* Ubuntu 14.04 and Debian 8
* Apache2 and Nginx
* Ansible 2.x

### Role Variables

#### Required

* `letsencrypt_domain` - Domain the certificate is for.
* `letsencrypt_email` - Your email as certificate owner.

#### Optional

* `letsencrypt_certbot_args` - Additional command line args to be passed to Certbot-- will be combined with `letsencrypt_certbot_default_args`. See [the Certbot docs](https://certbot.eff.org/docs/using.html) for arguments you may pass.
* `letsencrypt_certbot_default_args` - Please see `defaults/main.yml` what the default arguments are. Also, you could add To override all the arguments to Certbot, for example to use another plugin, set them using this variable.
* `letsencrypt_certbot_verbose` - Make Certbot output to console (default `true`).
* `letsencrypt_certbot_version` - Set specific Certbot version, for example a git tag or branch. Note that the lowest version of Certbot we support is 0.6.0.
* `letsencrypt_force_renew` - Whether to attempt renewal always, default to `true`.
* `letsencrypt_pause_services` - List of services to stop/start while calling Certbot.
* `letsencrypt_request_www` - Request `www.` automatically (default `true`).

### Example Playbook

This role works best when included just before your main site role, for example. Or it can be used in an individual playbook, for example as below.

This role should become root on the target host.

    ---
    - hosts: myhost
      become: yes
      become_user: root
      roles:
        - role: ansible-letsencrypt
          letsencrypt_email: email@example.com
          letsencrypt_domain: example.com
          letsencrypt_pause_services:
            - apache2

### License

MIT

### Author Information

Jason Robinson (@jaywink) - mail@jasonrobinson.me - https://iliketoast.net/u/jaywink - https://twitter.com/jaywink

Special thanks to Stefan Grönke (@gronke) for his work on expanding this role.

See CONTRIBUTORS for a full list of contributors.

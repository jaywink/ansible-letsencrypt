## [0.6.1] - 2017-01-27

### Fixed
* Fix regression relating to requesting `www.` domains introduced after 0.6.0. The regression caused a `www.` domain not to be requested even if explicitly setting `letsencrypt_request_www` to `true`. Note that this functionality used to work after the release of 0.6.0 AFAIK, but was possibly broken by a behaviour change in Ansible 2.1.

## [0.6.0] - 2016-11-19

### Important!
* Required Ansible version is now 2.x - see https://github.com/jaywink/ansible-letsencrypt/issues/20

### Added
- You can now specify all the parameters given to `certbot` by overriding the new variable `letsencrypt_certbot_default_args`. If you just want to add a parameter, use the old `letsencrypt_certbot_args` variable to add to the defaults already in place. Thanks for @robbyoconnor for this patch.

### Fixed
* Cloning Certbot from GitHub was using `depth: 1` for a quicker clone. This was causing problems in changing the version of Certbot later. Fixed by removing the `depth` argument. Thanks @brennen for reporting this issue.

## [0.5.0] - 2016-10-10

### Backwards incompatible changes
* Apache2 is no longer a dependency of this role and will not be installed. Thanks to @gronke for this patch. This also means `letsencrypt_pause_services` is an empty list by default. Make sure to add your webserver there so that it will be paused. A missing not installed service will not stop the role from executing so you can safely run this role before your main application role.

### Fixed
* Settings `letsencrypt_force_renew` to `false` caused Certbot to fail in some situations. Now this is fixed by passing Certbot the flag `--keep-until-expiring`, in the case that forced renewal is not desired. If the certificate is not due for renewal, nothing will be done by Certbot but no error will be raised either.

### Changed
* Certbot now runs with the `--non-interactive` flag, which should protect from Ansible hanging on unexpected prompts. **Note! This flag was added in Certbot 0.6.0** which is the lowest version this role can thus support.
* Default version of Certbot installed is now v0.8.1, the latest release as of now. Master branch can have unexpected breakages. Due to this, the cli flag `--no-self-upgrade` was also added to stop Certbot from automatically updating itself.

## [0.4.1] - 2016-09-04

### Fixed
* There was an error setting `letsencrypt_certbot_args` in 0.4.0. Thanks @gronke for a fast fix.

## [0.4.0] - 2016-09-03

### Added
* Allow configuring the certbot version with a new variable `letsencrypt_certbot_version`. This defaults to master. Thanks @gronke for this patch!
* Allow configuring what services are stopped when requesting a cert via new variable `letsencrypt_pause_services`. This is a list of items which by default includes `apache2`. You can set this variable empty to skip pausing services. Thanks @gronke for this patch!
* Allow configuring the `--renew-by-default` command line flag to Certbot. By default this is enabled, switch it off by setting `letsencrypt_force_renew` to `false`. Thanks @gronke.
* Additional Certbot command line args can now be passed in using the list variable `letsencrypt_certbot_args`. Thanks @gronke for the addition.

### Changed
* Stability changed to "beta" to be less scary :)

## [0.3.0] - 2016-06-29

### Added

* Allow specifying `letsencrypt_request_www` to disable requesting `www.` cert automatically. By default it is requested.

## [0.2.0] - 2016-05-14

### Added

* Automatically add a `www.` subdomain to the certificate.

### Fixed

* LetsEncrypt client is now Certbot. Adjusted this role to match the new renamed repository.

## [0.1.0] - 2016-05-02

Initial release.

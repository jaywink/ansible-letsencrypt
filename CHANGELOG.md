## [unreleased]

### Changed
* Certbot now runs with the `--non-interactive` flag, which should protect from Ansible hanging on unexpected prompts. **Note! This flag was added in Certbot 0.6.0** which is the lowest version this role can thus support.

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

# Trellis New Relic PHP Agent

[![Ansible Role](https://img.shields.io/ansible/role/20311.svg)](https://galaxy.ansible.com/TypistTech/trellis-newrelic-php/)
[![Build Status](https://travis-ci.org/TypistTech/trellis-newrelic-php.svg?branch=master)](https://travis-ci.org/TypistTech/trellis-newrelic-php)
[![GitHub tag](https://img.shields.io/github/tag/TypistTech/trellis-newrelic-php.svg)](https://github.com/TypistTech/trellis-newrelic-php/tags)
[![license](https://img.shields.io/github/license/TypistTech/trellis-newrelic-php.svg)](https://github.com/TypistTech/trellis-newrelic-php/blob/master/LICENSE)
[![Donate via PayPal](https://img.shields.io/badge/Donate-PayPal-blue.svg)](https://www.typist.tech/donate/sunny/)
[![Hire Typist Tech](https://img.shields.io/badge/Hire-Typist%20Tech-ff69b4.svg)](https://www.typist.tech/contact/)

Install [New Relic PHP agent](https://docs.newrelic.com/docs/agents/php-agent) on [Trellis](https://github.com/roots/trellis) servers

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Requirements](#requirements)
- [Installation](#installation)
- [Role Variables](#role-variables)
- [Hacking Trellis' Playbook](#hacking-trellis-playbook)
- [Common Errors](#common-errors)
  - [`vault_newrelic_license` is not defined](#vault_newrelic_license-is-not-defined)
- [See Also](#see-also)
- [Support!](#support)
  - [Donate via PayPal *](#donate-via-paypal-)
  - [Why don't you hire me?](#why-dont-you-hire-me)
  - [Want to help in other way? Want to be a sponsor?](#want-to-help-in-other-way-want-to-be-a-sponsor)
- [Feedback](#feedback)
- [Author Information](#author-information)
- [Contributing](#contributing)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Requirements

* [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html) 2.3 or later
* [Trellis](https://github.com/roots/trellis)
* [New Relic](https://newrelic.com/) account
* Ubuntu 16.04 (Xenial)

## Installation

Add this role to `requirements.yml`:

```yaml
- src: TypistTech.trellis-newrelic-php # Case-sensitive!
  version: 0.1.1 # Check for latest version!
```

Run `$ ansible-galaxy install -r requirements.yml` to install this new role.

## Role Variables

```yaml
# group_vars/<environment>/vault.yml
# This file should be encrypted. See: https://roots.io/trellis/docs/vault/
##########################################################################

# New Relic License Key
## See: https://docs.newrelic.com/docs/accounts-partnerships/accounts/account-setup/license-key
vault_newrelic_license: xxxxxxxxxxx

# group_vars/<environment>/main.yml
###################################

# Check for default values here: https://github.com/TypistTech/trellis-newrelic-php/blob/master/defaults/main.yml
# See also: https://docs.newrelic.com/docs/agents/php-agent/configuration/php-agent-configuration
newrelic_config:
  framework: wordpress
  appname: "My Awesome App {{ env }}"
  transaction_tracer.detail: 1
  datastore_tracer.database_name_reporting.enabled: true
```

## Hacking Trellis' Playbook

Add this role to `dev.yml` and `server.yml` **immediately after** `role: php`:

```yaml
roles:
    # Some other Trellis roles ...
    - { role: php, tags: [php] }
    - { role: TypistTech.trellis-newrelic-php, tags: [php, newrelic-php] }
    # Some other Trellis roles ...
```

## Common Errors

### `vault_newrelic_license` is not defined

Encrypt your New Relic license key in `group_vars/<environment>/vault.yml`. See [role variables](#role-variables).

### New Relic merges multiple environments into single application

Solution: Define different `appname` for different environments.

Tips:
```yaml
# group_vars/all/main.yml
#########################

newrelic_config:
  appname: "My Awesome App {{ env }}"
```

## Limitations

### Only one New Relic APM application per server

Pull requests are welcomed.

## See Also

* [New Relic PHP agent docs](https://docs.newrelic.com/docs/agents/php-agent)
* [Ansible Vault](https://roots.io/trellis/docs/vault/)

## Support!

### Donate via PayPal [![Donate via PayPal](https://img.shields.io/badge/Donate-PayPal-blue.svg)](https://www.typist.tech/donate/trellis-newrelic-php/)

Love Trellis New Relic PHP Agent? Help me maintain it, a [donation here](https://www.typist.tech/donate/trellis-newrelic-php/) can help with it.

### Why don't you hire me?

Ready to take freelance WordPress jobs. Contact me via the contact form [here](https://www.typist.tech/contact/) or, via email [info@typist.tech](mailto:info@typist.tech)

### Want to help in other way? Want to be a sponsor?

Contact: [Tang Rufus](mailto:tangrufus@gmail.com)

## Feedback

**Please provide feedback!** We want to make this library useful in as many projects as possible.
Please submit an [issue](https://github.com/TypistTech/trellis-newrelic-php/issues/new) and point out what you do and don't like, or fork the project and make suggestions.
**No issue is too small.**

## Author Information

[Trellis New Relic PHP Agent](https://github.com/TypistTech/trellis-newrelic-php) is a [Typist Tech](https://www.typist.tech) project and maintained by [Tang Rufus](https://twitter.com/Tangrufus), freelance developer for [hire](https://www.typist.tech/contact/).

Special thanks to [the Roots team](https://roots.io/about/) whose [Trellis](https://github.com/roots/trellis) make this project possible.

Full list of contributors can be found [here](https://github.com/TypistTech/trellis-newrelic-php/graphs/contributors).

## Contributing

Please see [CODE_OF_CONDUCT](./CODE_OF_CONDUCT.md) for details.

## License

[Trellis New Relic PHP Agent](https://github.com/TypistTech/trellis-newrelic-php) is released under the [MIT License](https://opensource.org/licenses/MIT).

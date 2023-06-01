# Trellis New Relic PHP Agent

[![Ansible Role](https://img.shields.io/ansible/role/20311?style=flat-square)](https://galaxy.ansible.com/TypistTech/trellis-newrelic-php)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/TypistTech/trellis-newrelic-php?style=flat-square)](https://github.com/TypistTech/trellis-newrelic-php/releases)
[![Ansible Role](https://img.shields.io/ansible/role/d/20311?style=flat-square)](https://galaxy.ansible.com/TypistTech/trellis-newrelic-php)
[![Ansible Quality Score](https://img.shields.io/ansible/quality/20311?style=flat-square)](https://galaxy.ansible.com/TypistTech/trellis-newrelic-php)
[![GitHub](https://img.shields.io/github/license/TypistTech/trellis-newrelic-php.svg)](https://github.com/TypistTech/trellis-newrelic-php/blob/master/LICENSE.md)
[![GitHub Sponsor](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa)](https://github.com/sponsors/TangRufus)
[![Sponsor via PayPal](https://img.shields.io/badge/Sponsor-PayPal-blue.svg)](https://typist.tech/donate/trellis-newrelic-php/)
[![Hire Typist Tech](https://img.shields.io/badge/Hire-Typist%20Tech-ff69b4.svg)](https://typist.tech/contact/)
[![Twitter Follow @TangRufus](https://img.shields.io/twitter/follow/TangRufus?style=flat-square&color=1da1f2)](https://twitter.com/tangrufus)


Install [New Relic PHP agent](https://docs.newrelic.com/docs/agents/php-agent) on [Trellis](https://github.com/roots/trellis) servers.

## Role Variables

```yaml
# group_vars/<environment>/vault.yml or host_vars/<host>/vault.yml
# This file should be encrypted. See: https://roots.io/trellis/docs/vault/
##########################################################################

# New Relic License Key
## See: https://docs.newrelic.com/docs/accounts-partnerships/accounts/account-setup/license-key
# vault_newrelic_license: false
vault_newrelic_license: xxxxxxxxxxx

# group_vars/<environment>/main.yml
###################################

# Indicates the desired package state.
# `latest` ensures that the latest version is installed.
# `present` does not update if already installed.
# Choices: present|latest
# Default: latest
newrelic_package_state: present

# Check for default values here: https://github.com/TypistTech/trellis-newrelic-php/blob/master/defaults/main.yml
# See also: https://docs.newrelic.com/docs/agents/php-agent/configuration/php-agent-configuration
newrelic_config:
  appname: "My Awesome App {{ env }}"
  framework: no_framework
  transaction_tracer.detail: 1
  datastore_tracer.database_name_reporting.enabled: true
```

## Hacking Trellis' Playbook

Add this role to `dev.yml` and `server.yml` **immediately after** `role: php`:

```diff
  # `dev.yml` & `server.yml`

  roles:
      # Some other Trellis roles ...
      - { role: php, tags: [php] }
+     - { role: TypistTech.trellis-newrelic-php, tags: [php, newrelic-php] }
      # Some other Trellis roles ...
```

## Requirements

* [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html) v2.7 or later
* Python v3.7.6 or later
* [Trellis](https://github.com/roots/trellis) v1.3.0 or later
* [New Relic](https://newrelic.com/) account

## Installation

Add this role to `galaxy.yml`:

```yaml
- src: TypistTech.trellis-newrelic-php # Case-sensitive!
  version: XXX.YYY.ZZZ # Check for latest version!
```

Run `$ trellis galaxy install` to install this new role.


## Multiple servers

As an alternative to setting the license key for an environment under `group_vars` you can set it for a single server under `host_vars`. You can explicitly set `false` to skip a group or host, or omit the variable from the vault file. A warning will be shown when setup is skipped.

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

### Error after upgrading PHP version

New Relic would fail and causes provision end up in errors when upgrading PHP major or minor releases (e.g: from 7.4 to 8.0, from 8.0 to 8.1).
```
non-zero return code
PHP Warning:  PHP Startup: Unable to load dynamic library 'newrelic.so'
(tried: /usr/lib/php/20180731/newrelic.so (/usr/lib/php/20180731/newrelic.so:
cannot open shared object file: No such file or directory),
/usr/lib/php/20180731/newrelic.so.so (/usr/lib/php/20180731/newrelic.so.so:
cannot open shared object file: No such file or directory)) in Unknown on
```

After each PHP major or minor release upgrade (i.e: when you see the above error):
```bash
# For multi-server setups, perform the following steps on each server.
ssh admin@123.456.789
sudo newrelic-install install
sudo reboot

# Wait for the server(s) to reboot and then re-provision
# For multi-server setups, you only need to re-provision once only
trellis provision production
```

## Limitations

* Only one New Relic APM application per server by default.

[`Extends`](https://jinja.palletsprojects.com/en/2.10.x/templates/#template-inheritance) the [`fastcgi_basic` block](https://github.com/roots/trellis/blob/73cbfb9ff840b7b55b60c77ee7d655c54211dbc1/roles/wordpress-setup/templates/wordpress-site.conf.j2#L236-L241) to include `fastcgi_param PHP_VALUE "newrelic.appname={{ item.key }} ({{ env }})";` right after the line `include fastcgi_params;` to work around this limitation.

Pull requests are welcomed.

## FAQs

### It looks awesome. Where can I find some more goodies like this?

* Articles on Typist Tech's [blog](https://typist.tech)
* [Tang Rufus' WordPress plugins](https://profiles.wordpress.org/tangrufus#content-plugins) on wp.org
* More projects on [Typist Tech's GitHub profile](https://github.com/TypistTech)
* Stay tuned on [Typist Tech's newsletter](https://typist.tech/go/newsletter)
* Follow [Tang Rufus' Twitter account](https://twitter.com/TangRufus)
* Hire [Tang Rufus](https://typist.tech/contact) to build your next awesome site

### This package isn't on wp.org. Where can I give a :star::star::star::star::star: review?

Thanks! Glad you like it. It's important to let me know somebody is using this project. Since this is not hosted on wordpress.org, please consider:

- :heart: [sponsor](https://github.com/sponsors/TangRufus) this project
- :star: star this [Github repo](https://github.com/TypistTech/trellis-newrelic-php)
- :eyes: [watch](https://github.com/TypistTech/trellis-newrelic-php/subscription) this Github repo
- tweet something good with mentioning [@TangRufus](https://twitter.com/tangrufus)
- write blog posts
- submit [pull requests](https://github.com/TypistTech/trellis-newrelic-php)
- [hire me](https://typist.tech/)

## Sponsoring :heart:

Love `trellis-newrelic-php`? Help me maintain it, a [sponsorship here](https://typist.tech/donation/) can help with it.

### GitHub Sponsors Matching Fund

Do you know [GitHub is going to match your sponsorship](https://help.github.com/en/github/supporting-the-open-source-community-with-github-sponsors/about-github-sponsors#about-the-github-sponsors-matching-fund)?

[Sponsor now via GitHub](https://github.com/sponsors/TangRufus) to double your greatness.

### Why don't you hire me?

Ready to take freelance WordPress jobs. Contact me via the contact form [here](https://typist.tech/contact/) or, via email [info@typist.tech](mailto:info@typist.tech)

### Want to help in other way? Want to be a sponsor?

Contact: [Tang Rufus](mailto:tangrufus@gmail.com)

## Feedback

**Please provide feedback!** We want to make this library useful in as many projects as possible.
Please submit an [issue](https://github.com/TypistTech/trellis-newrelic-php/issues/new) and point out what you do and don't like, or fork the project and make suggestions.
**No issue is too small.**

## Security

If you discover any security related issues, please email [trellis-newrelic-php@typist.tech](mailto:trellis-newrelic-php@typist.tech) instead of using the issue tracker.

## Credits

[Trellis New Relic PHP Agent](https://github.com/TypistTech/trellis-newrelic-php) is a [Typist Tech](https://typist.tech) project and maintained by [Tang Rufus](https://twitter.com/TangRufus), freelance developer for [hire](https://www.typist.tech/contact/).

Full list of contributors can be found [here](https://github.com/TypistTech/trellis-newrelic-php/graphs/contributors).

Special thanks to [the Roots team](https://roots.io/about/) whose [Trellis](https://github.com/roots/trellis) make this project possible.


## Contributing

Please see [CODE_OF_CONDUCT](./CODE_OF_CONDUCT.md) for details.

## License

[Trellis New Relic PHP Agent](https://github.com/TypistTech/trellis-newrelic-php) is released under the [MIT License](https://opensource.org/licenses/MIT).

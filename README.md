# vagrant-nodebugme

**CURRENTLY THIS IS NOT OPERATIONAL. I SPENT A COUPLE HOURS BUT STOPPED DUE TO LACK OF POSTGRESQL KNOWLEDGE/FRUSTRATION.**

Vagrant for [`nodebugme`][]

This was created to prevent `postgres` pollution on local machines.

[`nodebugme`]: https://github.com/nodebugme/site

## Getting Started
`vagrant-nodebugme` was developed against `Vagrant@1.4.1` but should be compatible with any `1.y.z`. To get `nodebugme` running, follow the steps below:

```bash
# Clone the repository
git clone https://github.com/twolfson/vagrant-nodebugme
cd vagrant-node

# Provision Vagrant
vagrant up

# SSH into the machine and start the server
# For development, you can edit `site/` on the host machine
vagrant ssh
  # Inside of Vagrant context
  cd /vagrant/site/
  npm start
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality.

## Donating
Support this project and [others by twolfson][gratipay] via [gratipay][].

[![Support via Gratipay][gratipay-badge]][gratipay]

[gratipay-badge]: https://cdn.rawgit.com/gratipay/gratipay-badge/2.x.x/dist/gratipay.png
[gratipay]: https://www.gratipay.com/twolfson/

## Unlicense
As of Oct 31 2014, Todd Wolfson has released this repository and its contents to the public domain.

It has been released under the [UNLICENSE][].

[UNLICENSE]: UNLICENSE

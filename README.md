# vagrant-nodebugme

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
  # starting server on http://localhost:8080
```

## Development
### git
`vagrant-nodebugme` clones the entire `nodebugme/site` repo into a `site/` in `vagrant-nodebugme's` directory. This is a fully funtional `git` repo that can check out branches, commit, and push.

### psql
`nodebugme/site` uses [`PostgreSQL`][] as its database. To debug database information, you can leverage the `psql` CLI tool. The database name will be `nodebugme`.

```bash
psql --db nodebugme
# nodebugme=#
```

If you are a first timer to `psql`, here are some quick gotchas/should knows:

- Always use single quotes when referring to data


- Results are not automatically sorted by index

[`PostgreSQL`]: http://www.postgresql.org/

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

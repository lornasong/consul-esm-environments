# consul-esm-environments
Spin up multiple environments with `consul` and `consul-esm` using Vagrant

Modified version of [Vagrantfile from Consul Demo](https://github.com/hashicorp/consul/blob/master/demo/vagrant-cluster/Vagrantfile)

**Version**

Use optional environment variables to specify versions of `consul` and `consul-esm`

E.G.
```
export CONSUL_VERSION=1.3.0
export CONSUL_ESM_VERSION=0.3.1
```
If environment variables are not specified, `consul` version defaults to most recent version and `consul-esm` to version 0.3.3

**Environments**

By default this will spin up 3 environments with hosts name n1, n2, n3

**CLI**

See [Vagrant documentary](https://www.vagrantup.com/docs/cli/) for more information.

To start up environments:
```
vagrant up
```

To tear down:
```
vagrant destroy
```

To SSH into environment:
```
vagrant ssh <hostname>
```
e.g.
```
vagrant ssh n1
```

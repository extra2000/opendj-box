# opendj-box

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/5w1ebua4gpuqni12/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/opendj-box/branch/master) |

Developer box for [OpenDJ](https://github.com/OpenIdentityPlatform/OpenDJ).


## Creating Vagrant Box

Copy example pillar files. Optionally you may want to edit the values in these files:
```
$ cp -v salt/roots/pillar/podman.sls.example salt/roots/pillar/podman.sls
$ cp -v salt/roots/pillar/opendj.sls.example salt/roots/pillar/opendj.sls
```

Copy vagrant file from `vagrant/examples/` and then create the vagrant box (you can change to `--provider=virtualbox` if you want to use Oracle VirtualBox provider):
```
$ cp -v vagrant/examples/Vagrantfile.opendj-box.fedora-34.x86_64.example vagrant/Vagrantfile.opendj-box
$ vagrant up --provider=libvirt
```

Provision the vagrant box:
```
$ vagrant ssh opendj-box -- sudo salt-call state.highstate
```

Deploy OpenDJ:
```
$ vagrant ssh opendj-box -- sudo salt-call state.sls opendj
```

## Test OpenDJ using cURL

```
$ vagrant ssh opendj-box -- curl -i ldap://127.0.0.1:1389
```


## Create systemd units for auto startup on boot

Generate systemd unit for OpenDJ and enable it:
```
$ cd ~/.config/systemd/user
$ podman generate systemd --files --name opendj-opendj-pod
$ systemctl --user enable pod-opendj-opendj-pod.service container-opendj-opendj-pod-opendj01.service
```

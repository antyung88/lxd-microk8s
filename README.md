# lxd-microk8s
Run Microk8s inside LXD/LXC Container

# Manual Installation

Create lxc microk8s profile
```
lxc profile create microk8s
```

Create lxc container
```
lxc launch -p default -p microk8s ubuntu:20.04 microk8s-1
```

lxc config edit
```
lxc config edit microk8s-1
```

Add the following lines under config
```
config:
  linux.kernel_modules: ip_tables,ip6_tables,netlink_diag,nf_nat,overlay
  raw.lxc: "lxc.apparmor.profile=unconfined\nlxc.cap.drop= \nlxc.cgroup.devices.allow=a\nlxc.mount.auto=proc:rw
    sys:rw"
  security.privileged: "true"
  security.nesting: "true"
```

Restart your lxc container
```
lxc exec microk8s-1 reboot
```

Get a shell in our container
```
lxc exec microk8s-1 -- /bin/bash
```

Install Microk8s via snap
```
snap install microk8s --classic
```

Check status:
```
microk8s inspect
```

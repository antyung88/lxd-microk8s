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

Add the following lines under devices
```
devices:
  aadisable:
    path: /sys/module/nf_conntrack/parameters/hashsize
    source: /sys/module/nf_conntrack/parameters/hashsize
    type: disk
  aadisable2:
    path: /dev/kmsg
    source: /dev/kmsg
    type: unix-char
  aadisable3:
    path: /sys/fs/bpf
    source: /sys/fs/bpf
    type: disk
  aadisable4:
    path: /proc/sys/net/netfilter/nf_conntrack_max
    source: /proc/sys/net/netfilter/nf_conntrack_max
    type: disk
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

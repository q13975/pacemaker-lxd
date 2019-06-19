# Pacemaker resource agent for LXD
This is a pacemaker resource agent for LXD, which I have used for years to failover lxc container in high HA cluster.  Please refer to the source code at [mylxd](https://github.com/q13975/articles/blob/master/mylxd).

Usage: 

- Download [mylxd](https://github.com/q13975/articles/blob/master/mylxd) and put the resource agent into /usr/lib/ocf/resource.d/hearbeat directory in all your HA cluster nodes.

- Configure the cluster for container 'test-container', which I assume you have set it up and disable its 'autostart' attribute in every HA cluster node:
```
  $ lxc config set test-container boot.autostart 0

  $ crm configure primitive test-lxd-cluster mylxd \
        params container="test-container" \
        op start timeout="60s" interval="0s" \
        op monitor timeout="30s" interval="10s" on-fail="restart" \
        op stop timeout="60s" interval="0s"
```

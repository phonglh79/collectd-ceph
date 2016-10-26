# Ubuntu Packaging

## Building the collectd-ceph package

[These instructions](http://packaging.ubuntu.com/html/packaging-new-software.html) should give full details.

In summary, do this once to prepare your environment:
```
pbuilder-dist precise create
```

and for every release (from master):
```
RELEASE=0.2.0
rm -rf /tmp/build-collectd
mkdir /tmp/build-collectd
cd /tmp/build-collectd
wget https://github.com/catalyst/collectd-ceph/archive/master.zip
unzip master.zip
tar zcvf collectd-ceph-${RELEASE}.tar.gz collectd-ceph-master/
bzr dh-make collectd-ceph ${RELEASE} collectd-ceph-${RELEASE}.tar.gz
cd collectd-ceph
bzr builddeb -S
cd ../build-area
pbuilder-dist precise build collectd-ceph_${RELEASE}-1ubuntu1.dsc
dput ppa:rocha-porto/collectd ../collectd-ceph_${RELEASE}-1ubuntu1_source.changes
```

## Rebuilding a newer collectd version for precise

```
bzr branch lp:ubuntu/saucy/collectd
sudo apt-get build-dep collectd
cd collectd/
dch -i
bzr bd -- -S -us -uc
cd ../
debsign collectd_5.1.0-3.1ubuntu4_source.changes
dput -f ppa:rocha-porto/collectd5-precise collectd_5.1.0-3.1ubuntu4_source.changes
```

## Rebuilding from another ppa

```
add-apt-repository ppa:user/ppa-name
apt-get source collectd=5.3.0
cd collectd-5.3.0/
dpkg-buildpackage -rfakeroot -d -us -uc -S
cd ..
debsign collectd_5.3.0_source.changes
dput -f ppa:rocha-porto/collectd5precise collectd_5.3.0_source.changes
```

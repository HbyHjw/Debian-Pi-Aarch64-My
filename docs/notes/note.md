#### kernel config

**CONFIG_CPU_SPECTRE=y  ?**

----

#### [ TIME ] Timed out waiting for device dev-ttyS0.device. [DEPEND] Dependency failed for Serial Getty on ttyS0.

A start job is running for dev-ttyS0.device

```
[ TIME ] Timed out waiting for device dev-ttyS0.device.
[DEPEND] Dependency failed for Serial Getty on ttyS0.
```

/lib/systemd/system/serial-getty@.service

## Steps

#### 1. First copy the template:

```
# cp /usr/lib/systemd/system/serial-getty@.service /etc/systemd/system/serial-getty@ttyS0.service
```

#### 2. Then edit the file and modify the agetty line:

```
[Service]
ExecStart=-/sbin/agetty --keep-baud 115200,38400,9600 %I $TERM    <-- Change this parameter
Type=idle
```

#### 3. Create a symlink:

```
# ln -s /etc/systemd/system/serial-getty@ttyS0.service /etc/systemd/system/getty.target.wants/
```

#### 4. Reload the daemon and start the service:

```
# systemctl daemon-reload
# systemctl start serial-getty@ttyS0.service
# systemctl enable serial-getty@ttyS0.service
```

----

## NoTe

#### for docker (riscv)

```
apt install -y conntrack ebtables ethtool iptables libip6tc2 libnetfilter-conntrack3 libnfnetlink0 libyajl-dev libyajl2 socat

maybe: update-alternatives --set iptables /usr/sbin/iptables-legacy
```

#### Debian ports for RISCV repo

```
deb http://ftp.ports.debian.org/debian-ports/ sid main
deb http://ftp.ports.debian.org/debian-ports/ unreleased main
```

```
deb https://mirrors.cloud.tencent.com/debian-ports/ sid main
deb https://mirrors.cloud.tencent.com/debian-ports/ unreleased main
```

```
deb https://mirrors.aliyun.com/debian-ports/ sid main
deb https://mirrors.aliyun.com/debian-ports/ unreleased main

#deb https://mirrors.aliyun.com/thead/debian-riscv64/ sid main

for thead need:
apt install gnupg2
wget https://mirrors.aliyun.com/thead/thead-keys/thead-repo.gpg
apt-key add ./thead-repo.gpg
rm -rf ./thead-repo.gpg

check:
apt-key list | grep 'F164 B941 4332 FC54'
```

#### vscode linux arm64

```
https://aka.ms/linux-arm64-deb
```

----

#### unixbench

```
UB_GCC_OPTIONS='-O3 -fomit-frame-pointer -fforce-addr -ffast-math -Wall -march=armv8-a -mtune=cortex-a72'
make
```

- stop system service not used
- Run script in /dev/shm

```
cat /sys/block/sda/queue/scheduler
echo xxx > /sys/block/sda/queue/scheduler
```

#### mksquashfs

```
mksquashfs squashfs-root xxx.img -comp xz -no-xattrs
```

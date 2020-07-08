# Simple NFS

- apple: `192.168.20.101`
- banana: `192.168.30.101`

## Apple

```bash
# sudo yum install nfs-utils;
sudo mkdir /var/nfsshare;
sudo chmod -R 755 /var/nfsshare;
sudo chown nfsnobody:nfsnobody /var/nfsshare;
echo '/var/nfsshare 192.168.*.*(rw,sync,no_root_squash,no_all_squash)' | sudo tee -a /etc/exports > /dev/null;
sudo exportfs -rav;
sudo systemctl enable rpcbind;
sudo systemctl enable nfs-server;
sudo systemctl enable nfs-lock;
sudo systemctl enable nfs-idmap;
sudo systemctl start rpcbind;
sudo systemctl start nfs-server;
sudo systemctl start nfs-lock;
sudo systemctl start nfs-idmap;

# sudo firewall-cmd --permanent --zone=public --add-service=nfs;
# sudo firewall-cmd --permanent --zone=public --add-service=mountd;
# sudo firewall-cmd --permanent --zone=public --add-service=rpc-bind;
# sudo firewall-cmd --reload;
```

## Banana

```bash
# sudo yum install nfs-utils;
sudo mkdir -p /mnt/nfs/var/nfsshare;
sudo mount -t nfs 192.168.20.101:/var/nfsshare /mnt/nfs/var/nfsshare;
```

```bash
df -kh

Filesystem                    Size  Used Avail Use% Mounted on
devtmpfs                      237M     0  237M   0% /dev
tmpfs                         244M     0  244M   0% /dev/shm
tmpfs                         244M  4.5M  240M   2% /run
tmpfs                         244M     0  244M   0% /sys/fs/cgroup
/dev/sda1                      40G  3.3G   37G   9% /
tmpfs                          49M     0   49M   0% /run/user/1000
192.168.20.101:/var/nfsshare   40G  3.3G   37G   9% /mnt/nfs/var/nfsshare
```

```bash
sudo vi /etc/fstab
```

```bash
192.168.20.101:/var/nfsshare /mnt/nfs/var/nfsshare nfs defaults 0 0
```

## Test

```bash
# apple
echo 'Hello' | sudo tee -a /var/nfsshare/apple.out > /dev/null;

# banana
cat /mnt/nfs/var/nfsshare/apple.out;
echo 'Bye' | sudo tee -a /mnt/nfs/var/nfsshare/banana.out > /dev/null;
```

## umount

```bash
sudo umount /mnt/nfs/var/nfsshare
sudo mount /mnt/nfs/var/nfsshare
```

# LVM Mirror
Quickstart and recovery LVM Mirror (RAID1) on Linux

## 1. Init Raid Device
#### 1.1. Create Raid device
```pvcreate /dev/sdb /dev/sdc```
#### 1.2. Create VG Raid
```vgcreate vgraid /dev/sdb /dev/sdc```
#### 1.3. Create LV Raid with mirroring
```lvcreate -n lvraid -m 1 -L100G --nosync vgraid```
#### 1.4. Check Raid State
```lvs```
#### 1.5. Format lv
```mkfs.ext4 /dev/vgraid/lvraid```
#### 1.6. Mount lv
```mount /dev/vgraid/lvraid /media/berto/lvraid```
<br />

## 2. Read Data From 1 Disk Only
#### 2.1. Remove Missing PV from VG
```vgreduce --removemissing vgraid --force```
#### 2.2. Convert Mirror 1 to Mirror 0
```lvconvert -m0 /dev/vgraid/lvraid -y```
#### 2.3. Mount LV With Single PV
```mount /dev/vgraid/lvraid /media/berto/lvraid```
<br />

## COPY NEW FILES ON LV
## INSERT NEW PV TO RESYNC
<br />

## 3. Recover Raid With New Device /dev/sdc:
#### Create PV
```pvcreate /dev/sdc```
#### 3.2. Re-add NEW PV to VG
```vgextend vgraid /dev/sdc```
#### 3.3. Ri-activate Mirror
```lvconvert -m1 /dev/vgraid/lvraid -y```


# pktgen_dpdk

## Grub

```bash
# Enable iommu in grub
# Follow https://github.com/EngrArsalanPervez/iommu_grub
```

## Bios

```bash
Enable IOMMU in BIOS
i.e., VT-d
```

## Installation

```bash
# Ubuntu 25.10
# C programming dependencies from eupnvim

git clone https://github.com/DPDK/dpdk.git
git checkout v25.11-rc4
meson setup build
cd build
ninja
sudo ninja install
sudo ldconfig

git clone https://github.com/pktgen/Pktgen-DPDK.git
git checkout pktgen-25.08.2
make
sudo make install
```

## pktgen.sh

```bash
# cd /home/network/Desktop/bootstartup
# nano pktgen.sh




#!/bin/bash
cd /home/network/Desktop/dpdk
modprobe uio
insmod ./dpdk-kmods/linux/igb_uio/igb_uio.ko
ifconfig ens785f0np0 down
ifconfig ens785f1np1 down
ifconfig ens785f2np2 down
ifconfig ens785f3np3 down
ifconfig ens786f0np0 down
ifconfig ens786f1np1 down
ifconfig ens786f2np2 down
ifconfig ens786f3np3 down
ifconfig ens802f0np0 down
ifconfig ens802f1np1 down
ifconfig ens802f2np2 down
ifconfig ens802f3np3 down
./usertools/dpdk-devbind.py --bind=igb_uio ens785f0np0
./usertools/dpdk-devbind.py --bind=igb_uio ens785f1np1
./usertools/dpdk-devbind.py --bind=igb_uio ens785f2np2
./usertools/dpdk-devbind.py --bind=igb_uio ens785f3np3
./usertools/dpdk-devbind.py --bind=igb_uio ens786f0np0
./usertools/dpdk-devbind.py --bind=igb_uio ens786f1np1
./usertools/dpdk-devbind.py --bind=igb_uio ens786f2np2
./usertools/dpdk-devbind.py --bind=igb_uio ens786f3np3
./usertools/dpdk-devbind.py --bind=igb_uio ens802f0np0
./usertools/dpdk-devbind.py --bind=igb_uio ens802f1np1
./usertools/dpdk-devbind.py --bind=igb_uio ens802f2np2
./usertools/dpdk-devbind.py --bind=igb_uio ens802f3np3
./usertools/dpdk-devbind.py -s
./usertools/dpdk-hugepages.py -p 1G -n 0 --setup 16G
./usertools/dpdk-hugepages.py -p 1G -n 1 --setup 16G
./usertools/dpdk-hugepages.py -s

cd /home/network/Desktop/Pktgen-DPDK
sudo ./tools/run.py default

#    'devices': (
#                '0000:05:00.0', '0000:05:00.1', '0000:05:00.2', '0000:05:00.3',
#                '0000:07:00.0', '0000:07:00.1', '0000:07:00.2', '0000:07:00.3',
#                '0000:82:00.0', '0000:82:00.1', '0000:82:00.2', '0000:82:00.3'
#            ),
#
#        'cores': '0-28',
#
#        'allowlist': (
#                '0000:05:00.0', '0000:05:00.1', '0000:05:00.2', '0000:05:00.3',
#                '0000:07:00.0', '0000:07:00.1', '0000:07:00.2', '0000:07:00.3',
#                '0000:82:00.0', '0000:82:00.1', '0000:82:00.2', '0000:82:00.3'
#                ),
#
#        'map': (
#                '[1:2].0',
#                '[3:4].1',
#                '[5:6].2',
#                '[7:8].3',
#                '[9:10].4',
#                '[11:12].5',
#                '[13:14].6',
#                '[15:16].7',
#                '[18:19].8',
#                '[20:21].9',
#                '[22:23].10',
#                '[24:25].11'
#                ),

```



## Pktgen default.cfg

```bash
#nano /home/network/Desktop/Pktgen-DPDK/cfg/default.cfg


description = 'A Pktgen default simple configuration'

# Setup configuration
setup = {
    'exec': (
        'sudo', '-E'
        ),

    'devices': (
                '0000:05:00.0', '0000:05:00.1', '0000:05:00.2', '0000:05:00.3',
                '0000:07:00.0', '0000:07:00.1', '0000:07:00.2', '0000:07:00.3',
                '0000:82:00.0', '0000:82:00.1', '0000:82:00.2', '0000:82:00.3'
            ),
    # UIO module type, igb_uio, vfio-pci or uio_pci_generic
    'uio': 'igb_uio'
    }

# Run command and options
run = {
    'exec': ('sudo', '-E'),

    # Application name and use app_path to help locate the app
    'app_name': 'pktgen',

    # using (sdk) or (target) for specific variables
    # add (app_name) of the application
    # Each path is tested for the application
    'app_path': (
                './usr/local/bin/%(app_name)s',
                '/usr/local/bin/%(app_name)s'
        ),

        'cores': '0-28',
        'nrank': '4',
        'proc': 'auto',
        'log': '7',
        'prefix': 'pg',

        'blocklist': (
                #'03:00.0', '05:00.0',
                #'81:00.0', '84:00.0'
                ),
        'allowlist': (
                '0000:05:00.0', '0000:05:00.1', '0000:05:00.2', '0000:05:00.3',
                '0000:07:00.0', '0000:07:00.1', '0000:07:00.2', '0000:07:00.3',
                '0000:82:00.0', '0000:82:00.1', '0000:82:00.2', '0000:82:00.3'
                ),

        'opts': (
                '-v',
                '-T',
                '-P',
                ),
        'map': (
                '[1:2].0',
                '[3:4].1',
                '[5:6].2',
                '[7:8].3',
                '[9:10].4',
                '[11:12].5',
                '[13:14].6',
                '[15:16].7',
                '[18:19].8',
                '[20:21].9',
                '[22:23].10',
                '[24:25].11'
                ),

        'theme': 'themes/black-yellow.theme',
        #'shared': '/usr/local/lib/x86_64-linux-gnu/dpdk/pmds-21.1'
        }






```



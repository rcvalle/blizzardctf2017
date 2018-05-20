Blizzard CTF 2017: Sombra True Random Number Generator (STRNG)
==============================================================

Sombra True Random Number Generator (STRNG) is a QEMU-based challenge I
developed for Blizzard CTF 2017. The challenge was to achieve a VM escape
from a QEMU-based VM and capture the flag located at /root/flag on the host.


Installation
------------

The image used and distributed with the challenge was the Ubuntu Server
14.04 LTS Cloud Image. The host used the same image as the guest. The guest
was reset every 10 minutes and was started with the following command:

    ./qemu-system-x86_64 \
        -m 1G \
        -device strng \
        -hda my-disk.img \
        -hdb my-seed.img \
        -nographic \
        -L pc-bios/ \
        -enable-kvm \
        -device e1000,netdev=net0 \
        -netdev user,id=net0,hostfwd=tcp::5555-:22

Access to the guest was provided by redirecting incoming connections to the
host on port 5555 to the guest on port 22. The guest login was "ubuntu" with
password "passw0rd".


Usage
-----

STRNG is a QEMU-based emulated hardware device. The device has four 32-bit
registers accessible through memory-mapped I/O and port-mapped I/O: rand's
seed (0), rand (1), rand_r's seed (2), and rand_r (3). These registers can
be accessed directly using memory-mapped I/O at BAR0 or indirectly through
the STRNG_PMIO_ADDR (0) and STRNG_PMIO_DATA (4) 32-bit ports using
port-mapped I/O at BAR1.

To generate a random number
1. Write the rand's seed to register 0.
2. Write to register 1 to call the device's rand implementation.
3. Read the generated random number from register 1.

Or
1. Write the rand_r's seed to register 2.
2. Write to register 3 to call the device's rand_r implementation.
3. Read the generated random number from register 3.
4. Optional: Read the state of rand_r from register 2.


Contributing
------------

See [CONTRIBUTING.md](CONTRIBUTING.md).


License
-------

Licensed under the MIT license. See [LICENSE](LICENSE) for license text and
copyright information.

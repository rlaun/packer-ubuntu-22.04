
# Build Ubuntu 22.04 with Packer

This project demonstrates usage of Packer with the new [Ubuntu autoinstaller](https://ubuntu.com/server/docs/install/autoinstall) which was introduced in Ubuntu 20.04 while the classic Debian Installer (d-i) using `preseed.conf` files has been deprecated.

The example uses QEMU/KVM as Packer Builder; you might want to use Vagrant/VirtualBox/etc. instead.

## Requirements

The following tools need to be installed on your Linux build host to build this example project:

* Packer (example config tested with 1.8.0)
* QEMU/KVM

## Usage

To build the VM with a QEMU window, run:

    $ packer build ubuntu-22.04.json

For a headless build (CI, ...) set the `is_headless` variable:

    $ packer build -var 'is_headless=true' ubuntu-22.04.json

## Pitfalls

Apparently Ubuntu needs a lot of resources while installing. Disk size under 5G or less than 1G RAM seems to result in crashes of the installer at various stages.

    "disk_size": "5000M",
    "memory": 2048,

Packer seems to have a [bug](https://github.com/hashicorp/packer-plugin-sdk/issues/87) that aborts the VM build after 2-5 minutes regardless of the `ssh_timeout` setting. As a workaround, set `ssh_handshake_attempts` to a high value.

    "ssh_timeout": "60m",            # ignored!?
    "ssh_handshake_attempts": 420    # workaround

Display `gtk` doesn't work on my system, YMMV.

    "display": "sdl",

## Further Reading

* Ubuntu autoinstall documentation: https://ubuntu.com/server/docs/install/autoinstall
* https://www.molnar-peter.hu/en/ubuntu-jammy-netinstall-pxe.html



## License 

MIT

## Shouts To

* [Jeff Geerling](https://jeffgeerling.com) for his [Packer Boxes](https://github.com/geerlingguy/packer-boxes) project which helped me a lot getting started with local VM builds using Packer some years ago
* [Molnár Péter](https://www.molnar-peter.hu) for his [excellent blog post about Ubuntu's autoinstaller](https://www.molnar-peter.hu/en/ubuntu-jammy-netinstall-pxe.html)

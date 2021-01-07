Compiling Redox
===============

Now we have prepared the build, so naturally we're going to build Redox.


```sh
$ make all
```

Give it a while. Redox is big.


Changing Redox Filesystem Size
------------------------------
"make all" generates a diskimage with the default filesystem size 256MB.

You are likely to want to increase this default size 256MB
edit the file redox/mk/config.mk
and change:
```sh
FILESYSTEM_SIZE?=256
```
To 20GB:
```sh
FILESYSTEM_SIZE?=20480
```

Then "make -B all" after that to ensure it rebuilds everything.

This will hopefully give you enough space to install
more packages inside this image for example:
```sh
$ pkg install gcc git ssh rust64
```


Running Redox
-------------

To run Redox, do:
```sh
$ make qemu
```

This should open up a Qemu window, booting to Redox.

If it does not work, try:

```sh
$ make qemu kvm=no # we disable KVM
```

or:

```sh
$ make qemu iommu=no
```

If this doesn't work either, you should go open an issue.

Note
----

If you encounter any bugs, errors, obstructions, or other annoying things, please report the issue to the [Redox repository]. Thanks!

[Redox repository]: https://gitlab.redox-os.org/redox-os/redox

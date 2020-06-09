# Switchtec-nvme-cli
This project is a fork of [nvme-cli][0] project with additional support of NVMe drives in the Switchtec Gen3 PAX fabric.

## Background
Switchtec Gen3 PAX is a variant of Microsemi's Switchtec PCIe switch product which enables the SR-IOV device sharing to mutiple hosts. Common SR-IOV devices include NICs, HBAs and IB cards, etc. Around early 2018, SR-IOV capable NVMe drives becomes available on market. With the support Switchtec Gen3 PAX switch, a SR-IOV capable NVMe drive can be managed by host and shared to multiple hosts.

<<<<<<< HEAD
By default, a NVMe drive in the PAX fabric cannot be enumerated by hosts. With the bind operation supported by the Switchtec Gen3 PAX switch, VFs can be bound to the hosts and be enumerated by the hosts.
=======
    $ make
    # make install
>>>>>>> e2126df6079466e80d76ce5fe86e4bed6a3b7028

Unlike some of other SR-IOV devices like NICs, the NVMe drives need to be properly configured before using. Normal configuration include create namespace and attache the namespace to a specific VF (NVMe secondary controller). And the configuration need to be done by the Fabric Manager.

<<<<<<< HEAD
Fabric Manager need a method to talk to the NVMe drives in the PAX fabric. Switchtec Gen3 PAX provides a special MRPC command to forward NVMe admin commands to the NVMe drives in the PAX fabric. The MRPC interface is a mechanism to allow hosts issue commands to Switchtec F/W. Two communication channels are provided between the Fabric Manager and the PAX switch, they are the inband PCIe channel and the MOE channel.
=======
    $ man nvme
>>>>>>> e2126df6079466e80d76ce5fe86e4bed6a3b7028

Switchtec-nvme-cli is responsible to create NVMe admin commands and deliver them to NVMe drives in PAX fabric through the special MRPC command, either over the inband PCIe channel or over the MOE channel.

<<<<<<< HEAD
For a better understanding of some terms of Switchtec PAX like MRPC, FABRIC MANAGER, MOE, please refer to Switchtec PAX device specification.
=======
    $ nvme help
>>>>>>> e2126df6079466e80d76ce5fe86e4bed6a3b7028

## Features
Swichtec-nvme-cli supports all features of nvme-cli for drives connected directly to host.

<<<<<<< HEAD
Swichtec-nvme-cli supports following features for NVMe drives behind PAX.
- List all NVMe devices and namespaces
- Create namespace
- Delete namespace
- Attaches a namespace to requested controller(s)
- Deletes a namespace from the controller
- Send NVMe Identify Controller
- Send NVMe Identify Namespace, display structure
- Send NVMe Identify List, display structure
=======
### Alpine Linux

nvme-cli is tested on Alpine Linux 3.3.  Install it using:

    # apk update && apk add nvme-cli nvme-cli-doc

if you just use the device you're after, it will work flawless.
```
# nvme smart-log /dev/nvme0
Smart Log for NVME device:/dev/nvme0 namespace-id:ffffffff
critical_warning                    : 0
temperature                         : 49 C
available_spare                     : 100%
```

### Arch Linux

nvme-cli is available in the `[community]` repository. It can be installed with:

    # pacman -S nvme-cli

The development version can be installed from AUR, e.g.:

    $ yay -S nvme-cli-git

### Fedora
>>>>>>> e2126df6079466e80d76ce5fe86e4bed6a3b7028

## Build and Installation
Switchtec-nvme-cli depends on the [switchtec-user][1] library and [switchtec-kernel][2] driver, among others. Please build and install the switchtec-user library and switchtec-kernel driver before building switchtec-nvme-cli.

<<<<<<< HEAD
To build and install this utility, simply run below commands. 
```
#sudo make
#sudo make install
```

## Examples
Here are some examples of managing the NVMe drives in a PAX fabric. For the NVMe drives connected directly to host, all commands from original nvme-cli are supported without any change.

### Fabric Manager over PCIe channel
1. List all NVMe devices and namespaces
```
#sudo ./nvme switchtec list
Node                       SN                   Model                                    Namespace Usage                      Format           FW Rev
-------------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
0x3300n1@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             1           6.55  MB /   6.55  MB    512   B +  0 B   REVISION
0x3300n2@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             2          13.11  MB /  13.11  MB    512   B +  0 B   REVISION
0x3300n3@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             3         131.07  MB / 131.07  MB    512   B +  0 B   REVISION
0x3300n4@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             4           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n5@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             5           2.15  GB /   2.15  GB    512   B +  0 B   REVISION

```
2. List controllers of a NVMe drive
```
#sudo ./nvme list-ctrl 0x3300@/dev/switchtec0
[   0]:0x1
[   1]:0x2
[   2]:0x3
[   3]:0x4
[   4]:0x5
[   5]:0x6
[   6]:0x7
[   7]:0x8
[   8]:0x9
[   9]:0xa
[  10]:0xb
[  11]:0xc
[  12]:0xd
[  13]:0xe
[  14]:0xf
[  15]:0x21
```
3. List namespaces of a NVMe drive
```
#sudo ./nvme list-ns 0x3300@/dev/switchtec0
[   0]:0x1
[   1]:0x2
[   2]:0x3
[   3]:0x4
[   4]:0x5
```
4. Create a 4GB namespace
```
#sudo ./nvme  create-ns 0x3300@/dev/switchtec0 -c 1048576 -s 1048576 -f 2
create-ns: Success, created nsid:6
```
5. Attach a namespace to a controller
```
#sudo ./nvme attach-ns 0x3300@/dev/switchtec0 -n 6 -c 0x21
attach-ns: Success, nsid:6
```
6. List all NVMe devices and namespaces
```
#sudo ./nvme switchtec list
Node                       SN                   Model                                    Namespace Usage                      Format           FW Rev
-------------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
0x3300n1@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             1           6.55  MB /   6.55  MB    512   B +  0 B   REVISION
0x3300n2@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             2          13.11  MB /  13.11  MB    512   B +  0 B   REVISION
0x3300n3@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             3         131.07  MB / 131.07  MB    512   B +  0 B   REVISION
0x3300n4@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             4           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n5@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             5           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n6@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             6           4.29  GB /   4.29  GB      4 KiB +  0 B   REVISION
```
7. Detach a namespace from a controller
```
#sudo ./nvme detach-ns 0x3300@/dev/switchtec0 -n 6 -c 0x21
detach-ns: Success, nsid:6
```
8. Delete a namespace
```
#sudo ./nvme delete-ns 0x3300@/dev/switchtec0 -n 6
delete-ns: Success, deleted nsid:6
```
9. List all NVMe devices and namespaces
```
#sudo ./nvme switchtec list
Node                       SN                   Model                                    Namespace Usage                      Format           FW Rev
-------------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
0x3300n1@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             1           6.55  MB /   6.55  MB    512   B +  0 B   REVISION
0x3300n2@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             2          13.11  MB /  13.11  MB    512   B +  0 B   REVISION
0x3300n3@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             3         131.07  MB / 131.07  MB    512   B +  0 B   REVISION
0x3300n4@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             4           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n5@/dev/switchtec0   SERIALNUMBER         VENDOR MODEL                             5           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
```

### Fabric Manager over MOE channel
1. List all NVMe devices and namespaces
```
#sudo ./nvme switchtec list -r 10.188.117.80:0
Node                       SN                   Model                                    Namespace Usage                      Format           FW Rev
-------------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
0x3300n1@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             1           6.55  MB /   6.55  MB    512   B +  0 B   REVISION
0x3300n2@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             2          13.11  MB /  13.11  MB    512   B +  0 B   REVISION
0x3300n3@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             3         131.07  MB / 131.07  MB    512   B +  0 B   REVISION
0x3300n4@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             4           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n5@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             5           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
```
2. List controllers of a NVMe drive
```
#sudo ./nvme list-ctrl 0x3300@10.188.117.80:0
[   0]:0x1
[   1]:0x2
[   2]:0x3
[   3]:0x4
[   4]:0x5
[   5]:0x6
[   6]:0x7
[   7]:0x8
[   8]:0x9
[   9]:0xa
[  10]:0xb
[  11]:0xc
[  12]:0xd
[  13]:0xe
[  14]:0xf
[  15]:0x21
```
3. List namespaces of a NVMe drive
```
#sudo ./nvme list-ns 0x3300@10.188.117.80:0
[   0]:0x1
[   1]:0x2
[   2]:0x3
[   3]:0x4
[   4]:0x5
```
4. Create a 4GB namespace
```
#sudo ./nvme  create-ns 0x3300@10.188.117.80:0 -c 1048576 -s 1048576 -f 2
create-ns: Success, created nsid:6
```
5. Attach a namespace to a controller
```
#sudo ./nvme attach-ns 0x3300@10.188.117.80:0 -n 6 -c 0x21
attach-ns: Success, nsid:6
```
6. List all NVMe devices and namespaces
```
#sudo ./nvme switchtec list -r 10.188.117.80:0
Node                       SN                   Model                                    Namespace Usage                      Format           FW Rev
-------------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
0x3300n1@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             1           6.55  MB /   6.55  MB    512   B +  0 B   REVISION
0x3300n2@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             2          13.11  MB /  13.11  MB    512   B +  0 B   REVISION
0x3300n3@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             3         131.07  MB / 131.07  MB    512   B +  0 B   REVISION
0x3300n4@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             4           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n5@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             5           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n6@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             6           4.29  GB /   4.29  GB      4 KiB +  0 B   REVISION
```
7. Detach a namespace from a controller
```
#sudo ./nvme detach-ns 0x3300@10.188.117.80:0 -n 6 -c 0x21
detach-ns: Success, nsid:6
```
8. Delete a namespace
```
#sudo ./nvme delete-ns 0x3300@10.188.117.80:0 -n 6
delete-ns: Success, deleted nsid:6
```
9. List all NVMe devices and namespaces
```
#sudo ./nvme switchtec list -r 10.188.117.80:0
Node                       SN                   Model                                    Namespace Usage                      Format           FW Rev
-------------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
0x3300n1@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             1           6.55  MB /   6.55  MB    512   B +  0 B   REVISION
0x3300n2@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             2          13.11  MB /  13.11  MB    512   B +  0 B   REVISION
0x3300n3@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             3         131.07  MB / 131.07  MB    512   B +  0 B   REVISION
0x3300n4@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             4           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
0x3300n5@10.188.117.80:0   SERIALNUMBER         VENDOR MODEL                             5           2.15  GB /   2.15  GB    512   B +  0 B   REVISION
=======
    $ sudo dnf install nvme-cli

### FreeBSD

`nvme-cli` is available in the FreeBSD Ports Collection.  A prebuilt binary
package can be installed with:

```console
# pkg install nvme-cli
```

### Gentoo

nvme-cli is available and tested in portage:
```
$ emerge -av nvme-cli
```

### Nix(OS)

The attribute is named `nvme-cli` and can e.g. be installed with:
```
$ nix-env -f '<nixpkgs>' -iA nvme-cli
```

### openSUSE

nvme-cli is available in openSUSE Leap 42.2 or later and Tumbleweed. You can install it using zypper.
For example:

    $ sudo zypper install nvme-cli

### Ubuntu

nvme-cli is supported in the Universe package sources for Xenial for
many architectures. For a complete list try running:
  ```
  rmadison nvme-cli
   nvme-cli | 0.3-1 | xenial/universe | source, amd64, arm64, armhf, i386, powerpc, ppc64el, s390x
  ```
A Debian based package for nvme-cli is currently maintained as a
Ubuntu PPA. Right now there is support for Trusty, Vivid and Wiley. To
install nvme-cli using this approach please perform the following
steps:
   1. Add the sbates PPA to your sources. One way to do this is to run
   ```
   sudo add-apt-repository ppa:sbates
   ```
   2. Perform an update of your repository list:
   ```
   sudo apt-get update
   ```
   3. Get nvme-cli!
   ```
   sudo apt-get install nvme-cli
   ```
   4. Test the code.
   ```
   sudo nvme list
   ```
   In the case of no NVMe devices you will see
   ```
   No NVMe devices detected.
   ```
   otherwise you will see information about each NVMe device installed
   in the system.

### OpenEmbedded/Yocto

An [nvme-cli recipe](https://layers.openembedded.org/layerindex/recipe/88631/)
is available as part of the `meta-openembeded` layer collection.

### Buildroot

`nvme-cli` is available as [buildroot](https://buildroot.org) package. The
package is named `nvme`.

### Other Distros

TBD

## Developers

You may wish to add a new command or possibly an entirely new plug-in
for some special extension outside the spec.

This project provides macros that help generate the code for you. If
you're interested in how that works, it is very similar to how trace
events are created by Linux kernel's 'ftrace' component.

### Add command to existing built-in

The first thing to do is define a new command entry in the command
list. This is declared in nvme-builtin.h. Simply append a new "ENTRY" into
the list. The ENTRY normally takes three arguments: the "name" of the 
subcommand (this is what the user will type at the command line to invoke
your command), a short help description of what your command does, and the
name of the function callback that you're going to write. Additionally,
You can declare an alias name of subcommand with fourth argument, if needed.

After the ENTRY is defined, you need to implement the callback. It takes
four arguments: argc, argv, the command structure associated with the
callback, and the plug-in structure that contains that command. The
prototype looks like this:

  ```c
  int f(int argc, char **argv, struct command *cmd, struct plugin *plugin);
  ```

The argc and argv are adjusted from the command line arguments to start
after the sub-command. So if the command line is "nvme foo --option=bar",
the argc is 1 and argv starts at "--option".

You can then define argument parsing for your sub-command's specific
options then do some command specific action in your callback.

### Add a new plugin

The nvme-cli provides macros to make define a new plug-in simpler. You
can certainly do all this by hand if you want, but it should be easier
to get going using the macros. To start, first create a header file
to define your plugin. This is where you will give your plugin a name,
description, and define all the sub-commands your plugin implements.

There is a very important order on how to define the plugin. The following
is a basic example on how to start this:

File: foo-plugin.h
```c
#undef CMD_INC_FILE
#define CMD_INC_FILE plugins/foo/foo-plugin

#if !defined(FOO) || defined(CMD_HEADER_MULTI_READ)
#define FOO

#include "cmd.h"

PLUGIN(NAME("foo", "Foo plugin"),
	COMMAND_LIST(
		ENTRY("bar", "foo bar", bar)
		ENTRY("baz", "foo baz", baz)
		ENTRY("qux", "foo quz", qux)
	)
);

#endif

#include "define_cmd.h"
```

In order to have the compiler generate the plugin through the xmacro
expansion, you need to include this header in your source file, with
pre-defining macro directive to create the commands.

To get started from the above example, we just need to define "CREATE_CMD"
and include the header:

File: foo-plugin.c
```c
#include "nvme.h"

#define CREATE_CMD
#include "foo-plugin.h"
>>>>>>> e2126df6079466e80d76ce5fe86e4bed6a3b7028
```

[0]: https://github.com/linux-nvme/nvme-cli
[1]: https://github.com/Microsemi/switchtec-user
[2]: https://github.com/Microsemi/switchtec-kernel

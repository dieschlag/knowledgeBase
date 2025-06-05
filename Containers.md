A container is a standard unit of software that packages up code and its dependencies, so the app can run on the host operating system. It does not have to include an OS, like [[Virtual Machine|VMs]] do.

Its memory footprint is lower and can be deployed much more easily. Also we can actually more containers than VMs on the same machine.

As they have to share the same host kernel, they are therefore less isolated than VM, but a trade of is that these containers can be hosted on VM, which are perfectly isloated.

It relies on specific primitives of the operating system (a specific data structure provided by the OS).

The first and only OS that provided with the necessary primitives to implement containers was Linux. These are the necessary elements:

- Namespaces: lightweight isolation to applications running on the same server
- Control Groups (cgroups): prevent an app from using all available ressources
- Union filesystem: used to create images for containers, as an assembly of different filesystems

An OS providing such elements is said to include native support for containers. Only Linux and Windows allow it.

To get through that difficulty, [[Container Runtime|Container Runtime]] was invented
A VM is a software that runs on a physical machine, as a guest of the physical machine. It has a full-blown OS which can be separated from the OS of the physical machine. The hardware of the machine is represented as virtual devices.

The physical ressources of a VM are allocated by an [[Hypervisor|hypervisor]].

Benefits of deploying on VMs is:

- Isolation: run on same server but are completely isolated between themselves
- Control over the ressources: cannot use more ressource than the one it was given. Hypervisors make sure than no VM is consuming all the ressources
- Encapsulation: Brings a standard way to start and stop a process , which is to start or stop the VM, regardless of the app it contains
- Cloud-native: it can be deployed in the could, and can benefit from their autoscaling functionnalities.

A VM is created via a template called an image, containing all thie files necessary to create and execute the VM, incuding the OS itself. As a consequence, can be of a large size and can take a while to start running.

A lighter solution is to use  [[Containers|containers]].
A hypervisor is a software component running on the host machine that allocates the host hardware computing ressources to the the guest VMs.

Architecture example for a Type 2 hypervisor (see definition below):

![[Pasted image 20250601150527.png]]

Two types of hypervisors:
- Type 1: bare-metal hypervisors: run directly on top of the host hardware and rely on the fact that the host processor supports virtualization. They are significantly more efficient than type 2.
- Type 2: corresponds to the example above. It runs on top of the operating  system as any other application. They don't have a direct access to the host machine hardware. It needs to send a system call to the operating system, which naturally introduces some latency.

Architecture example of a Type 1 hypervisor:

![[Pasted image 20250601150948.png]]
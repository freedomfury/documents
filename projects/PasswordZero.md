# Password Zero

When spinning up a compute node, the instance must have some access to an initial secret or privilege to unlock resources and data. This document refers to the initial secret as password zero. It solves the problem that might be trivial in the public cloud space, like AWS, using instance profiles with IAM roles. For our use case, we are using LXD/Incus hypervisor.  Since the hypervisor does not have this functionality built-in, we must develop a homegrown solution to solve this problem. As with everything else in this documentation, we want to solve it using the simplest solution. However, in some situations, complex complexity is unavoidable.


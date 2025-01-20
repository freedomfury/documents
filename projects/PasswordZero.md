# Password Zero

When spinning up a compute node, one must have access to an initial secret. This document refers to the initial secret as password zero. It tries to solve the problem that might be trivial in the public cloud space using instance profiles with IAM roles. For our use case, we are using LXD/Incus hypervisor.  Since the hypervisor does not have disability built in, we must come up with a homegrown solution to solving this problem. As with everything else in this documentation, we want to try to solve it using the simplest solution. However, in some situations, complex complexity is unavoidable.


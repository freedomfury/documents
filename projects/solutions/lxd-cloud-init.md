# Using cloud-init to set authentication credentials

Using cloud-init to set authentication credentials is relatively straightforward. The LXD hypervisor allows us to set profiles that store configuration and metadata about an instance. Cloud-init also enables you to insert arbitrary files. This functionality lets us insert the `password zero` secret to bootstrap the instance. This solution is also compatible with most public clouds, making it familiar to anyone with cloud-init experience.

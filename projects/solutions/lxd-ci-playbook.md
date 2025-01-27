# LXD cloud-init Ansible playbook
The playbook example bootstraps and initial Nginx web server for demonstration purposes. The example is purposely kept minimal to be used as a starting base for a more complicated setup.

> Full source code can be found here [main.yml](lxd-ci-playbook.yml).

Our project layout features a straightforward Ansible structure. The two most significant files are the main entry point playbook and the vault file. The vault file utilizes Ansible vault's built-in encryption mechanism. The file names are flexible and can be modified to fit your needs. The remainder of this document elaborates on the functionality within the `main.yml` file located at the root of our project.
```
├── files
│   ├── index.html
│   └── nginx.conf
├── group_vars
│   └── all
│       ├── main.yml
│       └── vault.yml      <-  ansible vault encrypted, secrets file
└── main.yml               <-  project main entry point playbook

3 directories, 5 files
```
More detailed documentation for Ansible Vault can be found on the vendor's website.

---

The default default unable host is set to all, matching any host in the inventory. Gathering facts set to `false` in our case, we do not use Ansible generated facts, and it slows down the execution process considerably. If Ansible facts are necessary, the `gather_subset` filters should be utilized to isolate the variables to be populated.
```
- name: "Example Nginx Web Server"
  hosts: all
  gather_facts: false
```
Run some initial miscellaneous setup tasks. This part of the example is somewhat contrived effectively. I want to show the use of a vaulted password variable, which would need the secret file to decrypt.
 ```
  tasks:
    # Misc prerequisites tasks ...
    - name: "Main | Output debug message"
      ansible.builtin.debug:
        msg: "Hello, world!"

    - name: "Main | Decrypted variable and write to file"
      ansible.builtin.copy:
        content: "{{ database_not_real_pwd }}"
        dest: /tmp/hello-world.txt
        mode: "0644"

    - name: "Main | install example packages"
      ansible.builtin.package:
        name: [which, tree]
        state: present
```
Next, we will install, configure, and start the web server processes. Basic web server configuration, nothing fancy.
```
 # Install and configure nginx web server ...
    - name: "Main | install nginx web server"
      ansible.builtin.package:
        name: nginx
        state: present

    - name: "Main | copy nginx config"
      ansible.builtin.copy:
        src: nginx.conf
        dest: /etc/nginx/nginx.conf
        owner: root
        group: root
        mode: "0644"

    - name: "Main | copy index html"
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/index.html
        owner: root
        group: root
        mode: "0644"

    - name: "Main | start nginx service"
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```
Clean up our secrets folder since they will no longer be needed, as this is a one-time bootstrap process. This step is important since we don't wanna leave these temporary sensitive files on the operating system.
```
    - name: "Main | Delete secrets folder"
      ansible.builtin.file:
        path: "/root/.sec"
        state: absent
```
Lastly, we run some validation to ensure the web server functions as expected. Testing as part of the playbook allows us to validate the bootstrap process in a single cloud-init. 
```
    # Test web server was setup correctly ...
    - name: "Main | check if port 80 is open"
      ansible.builtin.wait_for:
        host: localhost
        port: 80
        state: started
        delay: 5
        timeout: 10

    - name: "Main | Get content from web server"
      ansible.builtin.uri:
        url: "http://localhost:80"
        return_content: true
      register: webserver_content

    - name: "Main | Validate hello world string is in output"
      ansible.builtin.assert:
        that:
          - "'Hello, Nginx!' in webserver_content.content"
```
The command below demonstrates querying cloud-init status while waiting for completion from the hypervisor.
```
inc exec ct-ubun00-dev -- cloud-init status --wait

...............................................status: done
```
You will be presented with a progress bar and the run's final status.


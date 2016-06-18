Deploy monitoring
===

An [ansible](http://www.ansible.com) playbook for deploying ELK (pending) and Munin servers. Just drop-in an inventory and some minimal configuration then provision away =)

**Roles**

```bash
ansible-galaxy install -r requirements.yml --force --roles-path=roles/
```

**Local testing (with vagrant)**

```bash
vagrant up
```

The Munin web dashboard is available at `http://10.11.12.105/munin` (user: vagrant, password: vagrant).

**Dynamic inventory**

TODO: describe how to set master and add nodes dynamically. Nodes are required to have a `munin_group` variable to organize the nodes within Munin.

License
---

The project is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

---
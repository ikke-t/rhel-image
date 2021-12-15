# RHEL Image Builder Role

[![License: GPLv2](https://img.shields.io/badge/license-GPLv2-brightgreen.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Simple Ansible role for building RHEL images with Image Builder.

## Quick Usage Example

This role gets the image definition from git. So fix your git url
to point to image definitions repository, and edit your image
definition to your liking:

```
vi base-image.toml
git commit -m "Latest changes to base-image" base-image.toml
git push
```

Also install the role e.g. like this:
```
mkdir roles
cat > roles/requirements.yml<<EOF
---
roles:
  - src: https://github.com/ikke-t/rhel-image.git
    type: git
    version: onerole
EOF
ansible-galaxy role install -p roles -r roles/requirements.yml
```

Here is an [example playbook](./image_builder.yml) to use the role:


```
---

- name: build rhel image
  hosts: all
  become: true
  vars:
    git_remote_repo: file:///tmp/rhel-image-blueprints.git
    git_remote_repo_version: master
    image_blueprint: base-image
    image_size_kb: 20480
    image_output_type: qcow2
    update_host: false
    add_user: false

  roles:
    - rhel-image
```

Now you can run the playbook:

```
ansible-playbook -i 192.168.122.123, -u root image_builder.yml
```

or locally:

```
ansible-playbook -i localhost, -c local image_builder.yml
```

Next step for you likely is to have another tasks to upload and use the
created images.

## See Also

See also
[https://github.com/myllynen/ansible-packer](https://github.com/myllynen/ansible-packer).

## License

GPLv2+

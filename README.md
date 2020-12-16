Role Name
=========

A role for provisioning new Xen guests and configuring them with CloudInit.

Requirements
------------

Requires installation of the following Python packages:
 * [nocloud](https://pypi.org/project/nocloud/0.0.1/) for creating a [nocloud-compatible](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html) virtual disk image from the contents of a directory.
 * [pyvhd](https://pypi.org/project/pyvhd/) for creating virtual disk image files.

Role Variables
--------------
`deploy_environment`: A name for the deployment environment (e.g., "dev").

`xen_cluster_host`: Xen API endpoint. Typically, this is port 80 on a Xen server. This role defaults to `http://localhost:6781` to avoid certificate issues and assumes a local forward is configured.

`xen_cloudinit_directory`: Directory that this role will output CloudInit files to.
`xen_cloudinit_images_directory`: Directory that this role will output CloudInit disk images to.

`xen_username`: Username authorized to use Xen API.
`xen_password`: Password with for account authorized to use Xen API.

`xen_cloudinit_boot_disk_size`: Dictionary configuring primary boot disk size. See example.

`xen_cloudinit_cpus`: Number of VCPUs to allocate.
`xen_cloudinit_ram_mb`: MB of RAM to allocate.

`xen_cloudinit_username`: Default user for CloudInit.

`xen_cloudinit_preserve_apt`: Whether ot preserve apt sources.

`xen_cloudinit_user_data:`: Extra `user-data` file contents.

`xen_cloudinit_extra_roles`: Extra roles to load prior to provisioning VM. Can be used to configure roles that, for example, pre-configure a DNS auto-updater.

`xen_cloudinit_hostname`: Non-FQDN hostname. By default, this is extracted from the Ansible `inventory_hostname` variable.

`xen_cloudinit_sr`: Storage repository name to use on XenServer for boot disk and CloudInit disk image.

`xen_cloudinit_hosts`: List of hosts to provide static IP mappings for. Each list element is a line that will appear in `/etc/hosts`.

`xen_cloudinit_apt_mirror`: Mirror to use for apt.
`xen_cloudinit_apt_mirror_security`: Security mirror to use for apt.

Dependencies
------------

None.

Example Playbook
----------------


    - name: Provision test
      hosts: non
      # VMs may not exist yet, so there's nothing to gather facts from.
      gather_facts: no
      roles:
        - role: xen-cloudinit
          xen_cluster_host: "https://xenapi.example.com/"
          xen_username: "root"
          xen_cloudinit_sr: "sr01"
          xen_password: "{{lookup('community.general.keyring','xen-cluster root')}}"
          xen_cloudinit_networks:
            - name: "Lab servers"
          xen_cloudinit_boot_disk_size:
            unit: gb
            size: 3
          xen_cloudinit_cpus: 1
          xen_cloudinit_ram_mb: 512
          xen_cloudinit_user_data:
           - |
             bootcmd:
               - echo "Hello there"


License
-------

GPL 3.

Author Information
------------------

[jonathan lung](https://www.github.com/lungj)

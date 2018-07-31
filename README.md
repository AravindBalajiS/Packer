# Packer Builder for VMware vSphere

This Project helps in creating a VMware VM template for centos7 using packer.
Packer is an open source tool for creating identical machine images for multiple platforms from a single source configuration. More details can be found [packer.io](https://www.packer.io/intro)

## Installation(Packer)
 For installing packer in your machine do the following steps:
*   `sudo wget https://releases.hashicorp.com/packer/0.12.0/packer_0.12.0_linux_amd64.zip`
*   `sudo unzip packer_0.12.0_linux_amd64.zip -d packer`
*   `sudo mv packer /usr/local/`
*   `sudo gedit /etc/environment` and paste the following line at the end of the file
    `export PATH="$PATH:/usr/local/packer"` and save it.
*   `source /etc/environment`
*   Packer is now installed
*   To check packer is installed, type `packer`and you will see the following output.

  ```
    usage: packer [--version] [--help] <command> [<args>]

    Available commands are:
    build       build image(s) from template
    fix         fixes templates from old versions of packer
    inspect     see components of a template
    push        push a template and supporting files to a Packer build service
    validate    check that a template is valid
    version     Prints the Packer version
  ```


## Create the centos7 template

### Use the following steps to create centos7 template using Packer

* Clone this [repository](https://github.com/abalaji23/Packer.git) in the machine from which the template is to build and that has access to the ESXi Server.
* `git clone https://github.com/abalaji23/Packer.git`
* `cd Packer/Centos7/`
* `chmod +x packer-builder-vsphere-iso.linux`
*  Enter the network information in ks.cfg file.
*  Upload the ks.cfg in a private repository where it is accessible for the VMs inside the ESXi server and give the URL as a value in Centos7.json(boot_command).
*  Fill all the fields in Centos7.json file with the help of Parameter Reference given below.
* Execute the following command where your Centos7.json file is located
  ```
  packer build Centos7.json
  ```


## Parameter Reference

### Connection

* `vcenter_server`(string) - vCenter server hostname.
* `username`(string) - vSphere username.
* `password`(string) - vSphere password.
* `insecure_connection`(boolean) - Do not validate vCenter server's TLS certificate. Defaults to `false`.
* `datacenter`(string) - VMware datacenter name. Required if there is more than one datacenter in vCenter.

### VM Location

* `vm_name`(string) - Name of the new VM to create.
* `folder`(string) - VM folder to create the VM in.
* `host`(string) - ESXi host where target VM is created.

### Hardware

* `CPUs`(number) - Number of CPU sockets.
* `CPU_limit`(number) - Upper limit of available CPU resources in MHz.
* `ram`(number) - Amount of RAM in MB.
* `disk_size`(number) - The size of the disk in MB.
* `boot_order`(string) - Priority of boot devices. Defaults to `disk,cdrom`

### Hardware

* `guest_os_type`(string) - Set VM OS type. Defaults to `otherGuest`. See [here](https://pubs.vmware.com/vsphere-6-5/index.jsp?topic=%2Fcom.vmware.wssdk.apiref.doc%2Fvim.vm.GuestOsDescriptor.GuestOsIdentifier.html) for a full list of possible values.
* `network`(string) - Set network VM will be connected to.
* `network_card`(string) - Set VM network card type. Example `vmxnet3`.

### Boot

* `boot_command`(array of strings) - List of commands to type when the VM is first booted. Used to initialize the operating system installer.
* `iso_paths`(array of strings) - List of data store paths to ISO files that will be mounted to the VM. Example `"[datastore1] ISO/ubuntu-16.04.3-server-amd64.iso"`.

### Provision

* `ssh_username`(string) - Username in guest OS.
* `ssh_password`(string) - Password to access guest OS.

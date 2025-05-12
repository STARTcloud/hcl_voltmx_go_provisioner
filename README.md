<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/STARTcloud/hcl_voltmx_go_provisioner">
    <img src="https://raw.githubusercontent.com/STARTcloud/hcl_roles/refs/heads/main/roles/voltmx/files/voltmx.png" alt="HCL Volt MX Logo" width="468" height="131">
  </a>
</p>
<p align="center">
  <a href="https://github.com/STARTcloud/hcl_voltmx_go_provisioner/">
    <img src="https://startcloud.com/assets/images/logos/startCloud-logo-big.svg" alt="Startcloud Logo" width="468" height="131">
  </a>
</p>

  <h3 align="center">HCL VoltMX Go Provisioner</h3>

  <p align="center">
    A custom provisioner for Super.Human.Installer (SHI) to deploy HCL VoltMX Go for Domino using Ansible.
    <br />
    <a href="https://github.com/STARTcloud/hcl_voltmx_go_provisioner/"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/STARTcloud/hcl_voltmx_go_provisioner/">View Demo</a>
    ·
    <a href="https://github.com/STARTcloud/hcl_voltmx_go_provisioner/issues">Report Bug</a>
    ·
    <a href="https://github.com/STARTcloud/hcl_voltmx_go_provisioner/issues">Request Feature</a>
  </p>
</p>

<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About HCL VoltMX Go Provisioner](#about-hcl-voltmx-go-provisioner)
* [Key Features](#key-features)
* [Available Roles](#available-roles)
* [Galaxy Role File Structure](#galaxy-role-file-structure)
* [Integration with Super.Human.Installer (SHI)](#integration-with-superhumaninstaller-shi)
* [Interacting with SHI-generated `Hosts.yml`](#interacting-with-shi-generated-hostsyml)
* [Directory Structure Overview](#directory-structure-overview)
* [Roadmap](#roadmap)
* [Provider Support](#provider-support)
* [Built With](#built-with)
* [Contributing](#contributing)
* [License](#license)
* [Authors](#authors)
* [Acknowledgements](#acknowledgments)


## About HCL VoltMX Go Provisioner
The HCL VoltMX Go Provisioner is a custom provisioner designed for the Super.Human.Installer (SHI) application. It automates the deployment and configuration of HCL VoltMX Go for Domino, leveraging a Vagrant environment managed by SHI, and an underlying Ansible collection (`startcloud.hcl_roles`) to perform the detailed configuration tasks. This provisioner is related to the hcl_domino_standalone_provisioner and hcl_domino_additional_provisioner, utilizing similar structural capabilities for VoltMX Go deployment.

## Key Features

- **Super.Human.Installer Integration**: Seamlessly integrates as a custom provisioner within SHI, providing a user-friendly UI for configuration.
- **Comprehensive VoltMX Go Deployment**: Installs and configures all necessary components of the VoltMX Go platform.
- **Ansible-Powered Configuration**: Utilizes the `startcloud.hcl_roles` Ansible collection for robust and modular server configuration.
- **Automated Provisioning**: Simplifies the setup of all VoltMX Go components via SHI.
- **Domino Integration**: Sets up and configures HCL Domino for backend services.
- **VoltMX Go Configuration**: Manages the setup required by VoltMX Go.
- **Dependency Management**: Handles installation of necessary dependencies for each component.

## Available Ansible Roles (from `startcloud.hcl_roles` collection)

This provisioner utilizes the following Ansible roles from the `startcloud.hcl_roles` collection (located in `provisioners/ansible_collections/startcloud/hcl_roles/roles/`):

1.  **`voltmx`**:
    *   Description: Install and configure VoltMX for Domino.
    *   Dependencies:
        *   `startcloud.startcloud_roles.setup`
        *   `startcloud.startcloud_roles.dependencies`
        *   `startcloud.startcloud_roles.service_user`
    *   License: Apache

2.  **`domino_voltmx_go`**:
    *   Description: Manages the VoltMX Go for Domino setup.
    *   Dependencies:
        *   `startcloud.hcl_roles.domino_install`
        *   `startcloud.hcl_roles.voltmx`
    *   License: Apache

### Dependent Roles from `startcloud.hcl_roles`

The VoltMX Go roles depend on the following roles from the `startcloud.hcl_roles` collection:

1.  **`domino_install`**:
    *   Description: Installs and configures HCL Domino server.
    *   Dependencies:
        *   `startcloud.startcloud_roles.setup`
        *   `startcloud.startcloud_roles.dependencies`
        *   `startcloud.startcloud_roles.service_user`
    *   License: Apache

(Other `startcloud.hcl_roles` and `startcloud.startcloud_roles` are also dependencies but not detailed here as the focus is on the primary VoltMX Go components and their direct, significant dependencies.)

### Ansible Collection Structure (`startcloud.hcl_roles`)
The `startcloud.hcl_roles` Ansible collection, located at `provisioners/ansible_collections/startcloud/hcl_roles/`, follows the standard Ansible collection and role structure. Each role (e.g., `voltmx`) typically contains:
```
rolename/
├── defaults/         # Default variables for the role
│   └── main.yml
├── files/            # Static files to be copied to the remote host
├── handlers/         # Handlers, which are tasks triggered by notify directives
│   └── main.yml
├── meta/             # Role metadata (dependencies, galaxy info)
│   └── main.yml
├── tasks/            # Main list of tasks to be executed by the role
│   └── main.yml
├── templates/        # Template files (usually Jinja2)
└── vars/             # Variables for the role
    └── main.yml
```
For more details on the Ansible collection itself, see its README.md in the repository.

## Integration with Super.Human.Installer (SHI)

This `hcl_voltmx_go_provisioner` project is designed to function as a custom provisioner for the [Super.Human.Installer (SHI)](https://docs.superhumaninstaller.com/) application. For more details on how SHI handles custom provisioners, refer to the official SHI documentation, including how to [create custom provisioners](https://docs.superhumaninstaller.com/creating-custom-provisioners.html) and [import provisioners](https://docs.superhumaninstaller.com/provisioner-import.html).

Here's how the integration works:

1.  **SHI Custom Provisioner Structure**:
    *   This repository (`STARTcloud/hcl_voltmx_go_provisioner`) is structured as an SHI custom provisioner. For detailed information on the required structure and metadata files like `provisioner.yml` and `provisioner-collection.yml`, please see the [SHI documentation on Creating Custom Provisioners](https://docs.superhumaninstaller.com/creating-custom-provisioners.html).
    *   `./provisioner.yml` (at the root of this repository): This file defines the metadata for the "HCL VoltMX Go Provisioner" as it appears in SHI. It includes:
        *   User-configurable fields (e.g., hostname, domain, CPU, memory, various VoltMX and Domino specific parameters). These are presented in the SHI UI.
        *   A list of "roles" that can be toggled in the SHI UI (e.g., "VoltMX", "Domino VoltMX Go"). These toggles determine which parts of the Ansible automation are run.
    *   `./templates/Hosts.template.yml` (at the root of this repository): This is a crucial template used by SHI. When a user configures a new VoltMX Go server instance in SHI:
        *   SHI takes the user's input from the UI (defined by `./provisioner.yml`).
        *   It processes `./templates/Hosts.template.yml`, replacing placeholders with the actual configured values.
        *   The output is a `Hosts.yml` file, which is placed into the Vagrant environment for the new server instance.

2.  **Ansible Execution**:
    *   The generated `Hosts.yml` file serves as the primary source of variables for the Ansible playbooks run by Vagrant.
    *   The `provisioning.ansible.playbooks.local` section within `./templates/Hosts.template.yml` (and thus the generated `Hosts.yml`) defines which Ansible playbooks are run.
    *   These playbooks, in turn, execute the roles defined within the `startcloud.hcl_roles` Ansible collection (located at `./provisioners/ansible_collections/startcloud/hcl_roles/`).
    *   The Ansible roles (like `voltmx`, `domino_voltmx_go`, etc.) consume the variables that were originally set in the SHI UI and passed down through `./provisioner.yml` -> `./templates/Hosts.template.yml` -> generated `Hosts.yml`.

In essence, SHI provides the user interface and initial parameterization, `./provisioner.yml` defines what those parameters are, `./templates/Hosts.template.yml` bridges SHI's parameters to Ansible's variable system, and the `startcloud.hcl_roles` Ansible collection performs the actual configuration work on the target machine.

### Key Files for SHI Integration (within this `hcl_voltmx_go_provisioner` repository):

*   `./provisioner.yml`: Defines the SHI UI and available configuration options/roles.
*   `./templates/Hosts.template.yml`: SHI template to generate Ansible inventory/variables.
*   `./provisioners/ansible_collections/startcloud/hcl_roles/`: Contains the Ansible roles collection that performs the configuration.

## Interacting with SHI-generated `Hosts.yml`

The Ansible roles within the `startcloud.hcl_roles` collection are designed to be driven by variables defined in a `Hosts.yml` file that is generated by Super.Human.Installer based on user input and the `./templates/Hosts.template.yml` of this `hcl_voltmx_go_provisioner`.

When developing or modifying roles in the collection:
*   Refer to `./templates/Hosts.template.yml` to understand the structure of the variables that will be available to Ansible.
*   The `vars` section in `./templates/Hosts.template.yml` shows how SHI placeholders are mapped to Ansible variables.
*   The `roles` list in the Ansible provisioning section of `./templates/Hosts.template.yml` shows how SHI role toggles are used to conditionally run Ansible roles.

## Directory Structure Overview

```
hcl_voltmx_go_provisioner/
├── provisioner.yml                         # SHI Provisioner definition
├── templates/
│   └── Hosts.template.yml                  # SHI Template for Ansible variables
├── provisioners/
│   ├── ansible/                            # Ansible playbook configurations for Vagrant
│   │   ├── ansible.cfg
│   │   ├── generate-playbook.yml
│   │   ├── playbook.yml.j2
│   │   └── ...
│   └── ansible_collections/
│       └── startcloud/
│           └── hcl_roles/                  # The Ansible Collection
│               ├── README.md               # Collection-specific README
│               ├── galaxy.yml
│               ├── roles/                  # Individual Ansible roles
│               │   ├── voltmx/
│               │   ├── domino_voltmx_go/
│               │   └── ...
│               ├── CONTRIBUTING.md
│               ├── CODE_OF_CONDUCT.md
│               └── ...                     # Other collection-level files
├── .github/
│   └── PULL_REQUEST_TEMPLATE.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── LICENSE.md
├── README.md                               # This file
├── RELEASE.md
└── SECURITY.md
```

## Roadmap
See the [open issues](https://github.com/STARTcloud/hcl_voltmx_go_provisioner/issues) for a list of proposed features (and known issues).

## Provider Support

| Provider       | Supported by HCL VoltMX Go Provisioner |
|----------------|--------------------------------|
| VirtualBox     | Yes                            |
| Bhyve/Zones    | Yes                            |
| VMware Fusion  | No                             |
| Hyper-V        | No                             |
| Parallels      | No                             |
| AWS EC2        | Yes                            |
| Google Cloud   | No                             |
| Azure          | No                             |
| DigitalOcean   | No                             |
| Linode         | No                             |
| Vultr          | No                             |
| Oracle Cloud   | No                             |
| OpenStack      | No                             |
| Rackspace      | No                             |
| Alibaba Cloud  | No                             |
| Aiven          | No                             |
| Packet         | No                             |
| Scaleway       | No                             |
| OVH            | No                             |
| Exoscale       | No                             |
| Hetzner Cloud  | No                             |
| KVM            | Yes                            |
| QEMU           | Yes                            |
| Docker Desktop | No                             |
| HyperKit       | No                             |
| WSL2           | No                             |

## Built With
* [Vagrant](https://www.vagrantup.com/) - Portable Development Environment Suite.
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - Hypervisor.
* [Ansible](https://www.ansible.com/) - Virtual Machine Automation Management.
* [Core Provisioner](https://github.com/STARTcloud/core_provisioner) - Core Provisioner.
* [HCL Domino Standalone Provisioner](https://github.com/STARTcloud/hcl_domino_standalone_provisioner) - HCL Domino Standalone Provisioner.
* [HCL Domino Additional Provisioner](https://github.com/STARTcloud/hcl_domino_additional_provisioner) - HCL Domino Additional Provisioner.

## Contributing

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## License

This project is licensed under the SSLP v3 License - see the [LICENSE.md](LICENSE.md) file for details

## Authors
* **Joel Anderson** - *Initial work* - [JoelProminic](https://github.com/JoelProminic)
* **Justin Hill** - *Initial work* - [JustinProminic](https://github.com/JustinProminic)
* **Mark Gilbert** - *Refactor* - [MarkProminic](https://github.com/MarkProminic)

See also the list of [contributors](https://github.com/STARTcloud/hcl_voltmx_go_provisioner/graphs/contributors) who participated in this project.

## Acknowledgments

* Hat tip to anyone whose code was used

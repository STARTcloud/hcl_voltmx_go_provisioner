## Contributing to HCL VoltMX Go Provisioner

We welcome contributions to the HCL VoltMX Go Provisioner project! This project integrates with Super.Human.Installer (SHI) and utilizes the `startcloud.hcl_roles` Ansible collection. Please follow these guidelines to help us manage the project effectively.

### Getting Started

1.  **Fork the Repository**: Start by forking the [STARTcloud/hcl_voltmx_go_provisioner](https://github.com/STARTcloud/hcl_voltmx_go_provisioner) repository on GitHub.
2.  **Clone Your Fork**: Clone your forked repository to your local machine.
    ```bash
    git clone https://github.com/YOUR_USERNAME/hcl_voltmx_go_provisioner.git
    cd hcl_voltmx_go_provisioner
    ```
3.  **Create a Feature Branch**: Create a new branch for your changes.
    ```bash
    git checkout -b my-new-feature
    ```

### Making Changes

*   **Code Style**:
    *   For Ansible roles within the embedded collection (`provisioners/ansible_collections/startcloud/hcl_roles/`), ensure your code adheres to common best practices. We recommend using `ansible-lint`.
    *   For other parts of the provisioner (e.g., `provisioner.yml`, `templates/Hosts.template.yml`), maintain clarity and consistency.
*   **Commit Messages**: Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification for your commit messages. This helps in automating changelog generation and versioning for the overall project.
    *   Examples: `feat(provisioner): add new UI option for X`, `fix(ansible): correct typo in voltmx_go role`, `docs: update main README`
*   **Testing**:
    *   Test your changes by importing the provisioner into Super.Human.Installer and provisioning a server instance.
    *   If modifying Ansible roles, consider local testing with Molecule if applicable.
*   **Documentation**: If your changes affect user-facing options, SHI integration, or how the Ansible roles are used, please update the relevant documentation (e.g., `README.md`, `provisioner.yml` comments).

### Submitting a Pull Request

1.  **Push Your Changes**: Push your feature branch to your fork on GitHub.
    ```bash
    git push origin my-new-feature
    ```
2.  **Create a Pull Request**: Open a pull request from your feature branch to the `main` branch of the `STARTcloud/hcl_voltmx_go_provisioner` repository.
    *   Provide a clear title and description for your pull request, explaining the changes you've made and why.
    *   Reference any related issues (e.g., `Fixes #123`).
3.  **Code Review**: Your pull request will be reviewed by the maintainers. Be prepared to discuss your changes and make adjustments if necessary.
4.  **CI Checks**: Automated checks (e.g., linting, basic tests via GitHub Actions) will run on your pull request. Ensure these checks pass.

## Versioning

This project follows [Semantic Versioning 2.0.0](http://semver.org/).

## Development Environment

*   Super.Human.Installer (for testing the provisioner).
*   Vagrant and a supported hypervisor (e.g., VirtualBox).
*   Ansible (for working with the embedded Ansible collection).
*   `ansible-lint` (for linting Ansible roles).

## Releasing (For Maintainers)

The release process for the HCL VoltMX Go Provisioner typically involves:

1.  Ensuring all CI checks (if any) are passing on the `main` branch.
2.  Verifying that `CHANGELOG.md` (for the provisioner project) is up-to-date.
3.  Updating the version in `provisioner.yml` (e.g., `version: 0.1.24`) and potentially in the root `README.md` or other relevant files, according to SemVer.
4.  Creating a new Git tag for the release (e.g., `v0.1.24`).
5.  Pushing the tag to GitHub.
6.  Creating a corresponding release on GitHub with release notes.
    *   Package the provisioner (e.g., zip the `hcl_voltmx_go_provisioner` directory, excluding `.git` and other unnecessary files) and attach it to the GitHub release if distributing it as a downloadable artifact for SHI import.

Thank you for contributing!

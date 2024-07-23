# DVM Ansible Role

## Description

Opinionated Ansible role to install the DVM Project binaries on a site.

## Requirements

Debian or Ubuntu based system (including a Raspberry Pi), `apt`, and an AMD64, ARM64 or ARM32 CPU.

## Role Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `use_netbird` | Set this to true if Netbird is installed on the host | `false` | No |
| `dvm_folder` | Set this variable to use a custom folder name throughout the role | `dvm` | No |
| `rebuild` | Set this variable to force a rebuild using the GitHub action.  Rebuild will happen if the `dvmhost` project had changes since the last action run. *GitHub token is mandatory. See the Limitations section.* | `false` | No |
| `force_rebuild` | Set this variable to force the GitHub action to rebuild, regardless of whether the `dvmhost` project had changes since the last action run.  **`rebuild` *must* be set to `true` for this variable to be considered.** *GitHub token is mandatory. See the Limitations section.* | `false` | No |

## Limitations

- This role only supports P25.  While DVM supports DMR and NXDN, I did not add support for configuring them as part of this role.
- This role does not *yet* support DVRS mode.  Support for DVRS mode is planned in the future.

PRs are welcomed if you want to help add support for either of those things.

To use the `rebuild` and `force_rebuild` variables, a GitHub token is mandatory.  Since the [P25stuff/dvm-build](https://github.com/P25stuff/dvm-build) repo belongs to this org, this is not something you would have access to.  The appropriate way to accomplish this would be to fork or clone that repo under your own user, and then leverage your own token to trigger a build.  Without the token, the role will work except for dynamic rebuilds.

## Example Playbook

*Please refer to the [ansible-deploy repo](https://github.com/P25stuff/ansible-deploy/) for a detailed playbook.*

The role is configured in a way every single configuration item can be overridden by defining the appropriate variable, while maintaining most default values as a baseline.  See the table below for a list of required FNE variables.  In the `ansible-deploy` repo, those variables are provided in the `group_vars/all.yml` and `host_vars/fne.hostname.yml` files.  These can contain any variation of the variables to be defined (or any other accepted way to provide variables to Ansible).

In addition, this role expects files for RIDs, talkgroups, and peerlist (if applicable).  The information can be provided in any way, but for security purposes, the `ansible-deploy` repo relies on the info being in a private repository - [provided as an example](https://github.com/P25stuff/system-info/) in public form.

The role will create a new directory at `./local_backups/site-[siteID]/` and copy the generated configs locally upon running.

### Host Important Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `dvm_folder` | Set this variable to use a custom folder name throughout the role | `dvm` | Yes |
| `peer_id` | Must be set for each CC and VC. Sets the peer ID of that specific peer | None | Yes |
| `fne_address` | Hostname or IP for the system FNE | None | Yes |
| `fne_port` | Port number for the system FNE | None | Yes |
| `fne_password` | Password (or peer password) for the system FNE | None | Yes |
| `restAddress`, `restPort`, `restPassword` | Required for the REST API | None | Yes |
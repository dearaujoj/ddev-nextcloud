[![tests](https://github.com/dearaujoj/ddev-nextcloud/actions/workflows/tests.yml/badge.svg)](https://github.com/dearaujoj/ddev-nextcloud/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2024.svg)

# ddev-nextcloud <!-- omit in toc -->

* [What is ddev-nextcloud?](#what-is-ddev-nextcloud)
* [Components of the repository](#components-of-the-repository)
* [Getting started](#getting-started)
* [How to debug in Github Actions](#how-to-debug-tests-github-actions)

## What is ddev-nextcloud?

Nextcloud is a self-hosted file sharing and collaboration platform, similar to Dropbox or Google Drive, but with full control over your data.

### Core Features

- **File Management**
 - File sync across devices
 - Secure sharing capabilities
 - Version control

- **Collaboration Tools**
 - Calendar and contacts sync
 - Task management
 - Document collaboration
 - Video/audio chat

- **Extensibility**
 - App ecosystem for additional features
 - Password management
 - Notes and email integration

### Key Benefits

The platform prioritizes privacy and data sovereignty through self-hosting rather than relying on third-party cloud services.

> [Nextcloud.com](https://nextcloud.com/)

## Components of the repository

* A `docker-compose.nextcloud.yaml` that sets up Nextcloud with:
  * Automatic database integration
  * Persistent storage for data, config, and apps
  * Secure networking configuration
  * Health monitoring
* A `config.nextcloud-db.yaml` for database configuration
* An `install.yaml` that manages the addon installation with:
  * Pre-installation checks
  * Database creation and configuration
  * Post-installation instructions
  * Clean removal process including database cleanup
* Test suite in `tests/test.bats` to ensure reliability
* GitHub Actions setup in `.github/workflows/tests.yml` for automated testing

## Getting started

### For Users

1. Installation:

    For DDEV v1.23.5 or above run

    ```bash
    ddev add-on get dearaujoj/ddev-nextcloud && ddev restart
    ```

    For earlier versions of DDEV run

    ```bash
    ddev get dearaujoj/ddev-nextcloud && ddev restart
    ```

2. Access Nextcloud at: `https://<projectname>.ddev.site`

3. Log in with the default credentials:
   * Username: `admin`
   * Password: `admin`

### For Contributors

1. Fork the repository
2. Create your feature branch
3. Test your changes:
   ```bash
   ddev start
   bats tests
   ```
4. Submit a pull request

## How to debug tests (Github Actions)

1. You need an SSH-key registered with GitHub. You either pick the key you have already used with `github.com` or you create a dedicated new one with `ssh-keygen -t ed25519 -a 64 -f tmate_ed25519 -C "$(date +'%d-%m-%Y')"` and add it at `https://github.com/settings/keys`.

2. Add the following snippet to `~/.ssh/config`:

```
Host *.tmate.io
    User git
    AddKeysToAgent yes
    UseKeychain yes
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/tmate_ed25519
```
3. Go to `https://github.com/<user>/<repo>/actions/workflows/tests.yml`.

4. Click the `Run workflow` button and you will have the option to select the branch to run the workflow from and activate `tmate` by checking the `Debug with tmate` checkbox for this run.

![tmate](images/gh-tmate.jpg)

5. After the `workflow_dispatch` event was triggered, click the `All workflows` link in the sidebar and then click the `tests` action in progress workflow.

7. Pick one of the jobs in progress in the sidebar.

8. Wait until the current task list reaches the `tmate debugging session` section and the output shows something like:

```
106 SSH: ssh PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
107 or: ssh -i <path-to-private-SSH-key> PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
108 SSH: ssh PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
109 or: ssh -i <path-to-private-SSH-key> PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io
```

9. Copy and execute the first option `ssh PRbaS7SLVxbXImhjUqydQBgDL@nyc1.tmate.io` in the terminal and continue by pressing either <kbd>q</kbd> or <kbd>Ctrl</kbd> + <kbd>c</kbd>.

10. Start the Bats test with `bats ./tests/test.bats`.

For a more detailed documentation about `tmate` see [Debug your GitHub Actions by using tmate](https://mxschmitt.github.io/action-tmate/).

**Contributed and maintained by [@CONTRIBUTOR](https://github.com/dearaujoj)**

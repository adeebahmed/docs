---
title: "Git"
---

## Overview

{{% alert color="info" %}}
This plugin is automatically injected into every pipeline for the source repository.
{{% /alert %}}

This plugin enables the ability to clone repositories in a Vela pipeline to the build workspace.

Source Code: https://github.com/go-vela/vela-git

Registry: https://hub.docker.com/r/target/vela-git

## Usage

{{% alert color="warning" %}}
Users should refrain from using `latest` as the tag for the Docker image.

It is recommended to use a semantically versioned tag instead.
{{% /alert %}}

Sample of cloning a repository:

```yaml
steps:
  - name: clone_hello-world
    image: target/vela-git:latest
    pull: always
    parameters:
      path: hello-world
      ref: refs/heads/master
      remote: https://github.com/octocat/hello-world.git
      sha: 7fd1a60b01f91b314f59955a4e4d4e80d8edf11d
```

Sample of cloning a repository with submodules:

```diff
steps:
  - name: clone_hello-world
    image: target/vela-git:latest
    pull: always
    parameters:
      path: hello-world
      ref: refs/heads/master
      remote: https://github.com/octocat/hello-world.git
      sha: 7fd1a60b01f91b314f59955a4e4d4e80d8edf11d
+     submodules: true
```

Sample of cloning a repository with tags:

```diff
steps:
  - name: clone_hello-world
    image: target/vela-git:latest
    pull: always
    parameters:
      path: hello-world
      ref: refs/heads/master
      remote: https://github.com/octocat/hello-world.git
      sha: 7fd1a60b01f91b314f59955a4e4d4e80d8edf11d
+     tags: true
```

## Secrets

{{% alert color="warning" %}}
Users should refrain from configuring sensitive information in their pipeline in plain text.
{{% /alert %}}

### Internal

The plugin accepts the following `parameters` for authentication:

| Parameter        | Environment Variable Configuration                                |
| ---------------- | ----------------------------------------------------------------- |
| `netrc_password` | `GIT_PASSWORD`, `PARAMETER_NETRC_PASSWORD`, `VELA_NETRC_PASSWORD` |
| `netrc_username` | `GIT_USERNAME`, `PARAMETER_NETRC_USERNAME`, `VELA_NETRC_USERNAME` |

Users can use [Vela internal secrets](/docs/concepts/pipeline/secrets/) to substitute sensitive values at runtime:

```diff
steps:
  - name: clone_hello-world
    image: target/vela-git:latest
    pull: always
+   secrets: [ git_username, git_password ]
    parameters:
-     netrc_username: octocat
-     netrc_password: superSecretPassword
      path: /vela/src/github.com/octocat/hello-world
      ref: refs/heads/master
      remote: https://github.com/octocat/hello-world.git
      sha: 7fd1a60b01f91b314f59955a4e4d4e80d8edf11d
```

{{% alert color="info" %}}
This example will add the `secrets` to the `clone_hello-world` step as environment variables:

- `GIT_USERNAME=<value>`
- `GIT_PASSWORD=<value>`
{{% /alert %}}

### External

The plugin accepts the following files for authentication:

| Parameter        | Volume Configuration                                                      |
| ---------------- | ------------------------------------------------------------------------- |
| `netrc_password` | `/vela/parameters/git/netrc/password`, `/vela/secrets/git/netrc/password` |
| `netrc_username` | `/vela/parameters/git/netrc/username`, `/vela/secrets/git/netrc/username` |

Users can use [Vela external secrets](/docs/concepts/pipeline/secrets/) to substitute these sensitive values at runtime:

```diff
steps:
  - name: clone_hello-world
    image: target/vela-git:latest
    pull: always
    parameters:
-     netrc_username: octocat
-     netrc_password: superSecretPassword
      path: /vela/src/github.com/octocat/hello-world
      ref: refs/heads/master
      remote: https://github.com/octocat/hello-world.git
      sha: 7fd1a60b01f91b314f59955a4e4d4e80d8edf11d
```

{{% alert color="info" %}}
This example will read the secrets values in the volume stored at `/vela/secrets/`:
{{% /alert %}}

## Parameters

{{% alert color="info" %}}
The plugin supports reading all parameters vie environment variables or files.

Any values set from a file take precedence over values set from the environment.
{{% /alert %}}

The following parameters are used to configure the image:

| Name             | Description                       | Required | Default             |
| ---------------- | --------------------------------- | -------- | ------------------- |
| `log_level`      | set the log level for the plugin  | `true`   | `info`              |
| `netrc_machine`  | machine name to communicate with  | `true`   | `github.com`        |
| `netrc_password` | password for authentication       | `true`   | **set by Vela**     |
| `netrc_username` | user name for authentication      | `true`   | **set by Vela**     |
| `path`           | local path to clone repository to | `true`   | **set by Vela**     |
| `ref`            | reference generated for commit    | `true`   | `refs/heads/master` |
| `remote`         | full url for cloning repository   | `true`   | **set by Vela**     |
| `sha`            | SHA-1 hash generated for commit   | `true`   | **set by Vela**     |
| `submodules`     | enables fetching of submodules    | `false`  | `false`             |
| `tags`           | enables fetching of tags          | `false`  | `false`             |

## Template

COMING SOON!

## Troubleshooting

You can start troubleshooting this plugin by tuning the level of logs being displayed:

```diff
steps:
  - name: clone_hello-world
    image: target/vela-git:latest
    pull: always
    parameters:
+     log_level: trace
      path: hello-world
      ref: refs/heads/master
      remote: https://github.com/octocat/hello-world.git
      sha: 7fd1a60b01f91b314f59955a4e4d4e80d8edf11d
```

Below are a list of common problems and how to solve them:

COMING SOON!
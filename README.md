# :truck: camion.sh

Shell script to deploy web app to server.

## Install

```shell
sudo wget https://raw.githubusercontent.com/webzhao/camion.sh/master/camion -P /usr/local/bin && sudo chmod a+x /usr/local/bin/camion
```

## Configuration

By default `camion.sh` will look for configuration file `deploy.ini` in the **project root directory**. Run `camion init` to generate a sample configuration file.

`deploy.ini` consists of one or more environments, such as `[test]`, `[production]`, etc. Here is a sample configuration file for testing environment:

```ini
[test]
project = myapp
user    = work
files   = app src resource
dir     = /data/www/myapp
hosts   = 10.0.0.1
```

If some properties in multiple environments have same values, you can place them to `[common]` section:

```ini
[common]
project = myapp
user    = work
dir     = /data/www/myapp

[test]
files   = app src resource
hosts   = 10.0.0.1

[production]
files   = app pm2.json
hosts   = 10.0.0.2 10.0.0.3
```

| Property  | Required  | Description                                       |
|-----------|-----------|---------------------------------------------------|
| project   | yes       | project name                                      |
| user      | yes       | user used to sync files to server, see note below |
| dir       | yes       | directory the project should be deployed to       |
| files     | yes       | files which should be deployed to server          |
| hosts     | yes       | server IPs or hostnames, seperated by space       |
| prepare   | no        | commands to execute before everything start       |
| before    | no        | commands to execute before sync files to server   |
| after     | no        | commands to execute **ON SERVER** after deployment|
| clean     | no        | commands to execute after deployment              |

> NOTE: `user` configured in `deploy.ini` must have permission to access servers using SSH key.

## Usage

Run `camion <command> [env]` where `<command>` is one of `init`, `deploy`, `list`, `revert` and `update`,  `[env]` is one of the environment in your configuration file.

#### init

To generate a sample `deploy.ini` configuration file in current directory.

#### deploy

Deploy current version to specified environment, eg `camion deploy production`. Environment is required for this command.

#### list

List deploy history for current project and environment. Environment is required for this command.

#### revert

Revert to previous deployed version. Environment is required for this command.

#### update

Upgrade `camion.sh` to latest version. Root privilege is required.


## Utilities

There are some built-in utilities you can use in `prepare`, `before` hook.

#### assertGitUpdated

To ensure local version is up to date.

#### assertGitClean

To ensure everything is pushed.



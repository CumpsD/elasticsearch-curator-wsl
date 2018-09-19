# elasticsearch-curator-wsl

### Download and install the Public Signing Key:

```bash
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

### Add the following path in your /etc/apt/sources.list.d/ directory in a file, curator.list

`deb [arch=amd64] https://packages.elastic.co/curator/5/debian stable main`

```bash
sudo vim.tiny /etc/apt/sources.list.d/curator.list
```

### Install elasticsearch-curator

```bash
sudo apt-get update && sudo apt-get install elasticsearch-curator
```

### Create a file, curator.yml that contains the elasticsearch server configuration

```bash
vim.tiny curator.yml
```

```yaml
---
# Remember, leave a key empty if there is no value.  None will be a string,
# not a Python "NoneType"
client:
  hosts:
    - [host]
  port: [port]
  url_prefix:
  use_ssl: True
  certificate:
  client_cert:
  client_key:
  aws_key:
  aws_secret_key:
  aws_region:
  ssl_no_validate: False
  http_auth: [user]:[pass]
  timeout: 30
  master_only: False

logging:
  loglevel: INFO
  logfile:
  logformat: default
  blacklist: ['elasticsearch', 'urllib3']

```

### Create a file, close.yml that contains all configuration for the action you want to execute

In this case, we want to close all indices that match the `logstash-dev-` pattern and are older than 5 days.

```bash
sudo vim.tiny close.yml
```

```yaml
---
# Remember, leave a key empty if there is no value.  None will be a string,
# not a Python "NoneType"
#
# Also remember that all examples have 'disable_action' set to True.  If you
# want to use this action as a template, be sure to set this to False after
# copying it.
actions:
  1:
    action: close
    description: >-
      Close indices older than 5 days (based on index name), for logstash-dev- prefixed indices.
    options:
      delete_aliases: False
      timeout_override:
      continue_if_exception: False
      disable_action: False
    filters:
    - filtertype: pattern
      kind: prefix
      value: logstash-dev-
      exclude:
    - filtertype: age
      source: name
      direction: older
      timestring: '%Y.%m.%d'
      unit: days
      unit_count: 5
      exclude:

```

### Execute the command

The `--dry-run` prevents the action from being applied.

```bash
curator --config curator.yml --dry-run close.yml
```

Should given the following output:

```bash
2018-09-19 16:26:45,589 INFO      Preparing Action ID: 1, "close"
2018-09-19 16:26:46,026 INFO      Trying Action ID: 1, "close": Close indices older than 30 days (based on index name), for logstash- prefixed indices.
2018-09-19 16:26:46,450 INFO      DRY-RUN MODE.  No changes will be made.
2018-09-19 16:26:46,450 INFO      (CLOSED) indices may be shown that may not be acted on by action "close".
2018-09-19 16:26:46,450 INFO      DRY-RUN: close: logstash-2018.09.01 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,451 INFO      DRY-RUN: close: logstash-2018.09.02 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,451 INFO      DRY-RUN: close: logstash-2018.09.03 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,452 INFO      DRY-RUN: close: logstash-2018.09.04 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,452 INFO      DRY-RUN: close: logstash-2018.09.05 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,452 INFO      DRY-RUN: close: logstash-2018.09.06 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,452 INFO      DRY-RUN: close: logstash-2018.09.07 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,453 INFO      DRY-RUN: close: logstash-2018.09.08 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,453 INFO      DRY-RUN: close: logstash-2018.09.09 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,453 INFO      DRY-RUN: close: logstash-2018.09.10 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,453 INFO      DRY-RUN: close: logstash-2018.09.11 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,453 INFO      DRY-RUN: close: logstash-2018.09.12 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,453 INFO      DRY-RUN: close: logstash-2018.09.13 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,454 INFO      DRY-RUN: close: logstash-2018.09.14 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,454 INFO      DRY-RUN: close: logstash-beta-2018.09.01 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,454 INFO      DRY-RUN: close: logstash-beta-2018.09.02 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,454 INFO      DRY-RUN: close: logstash-beta-2018.09.03 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,454 INFO      DRY-RUN: close: logstash-beta-2018.09.04 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,454 INFO      DRY-RUN: close: logstash-beta-2018.09.05 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,455 INFO      DRY-RUN: close: logstash-beta-2018.09.06 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,455 INFO      DRY-RUN: close: logstash-beta-2018.09.07 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,455 INFO      DRY-RUN: close: logstash-beta-2018.09.08 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,455 INFO      DRY-RUN: close: logstash-beta-2018.09.09 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,456 INFO      DRY-RUN: close: logstash-beta-2018.09.10 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,456 INFO      DRY-RUN: close: logstash-beta-2018.09.11 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,456 INFO      DRY-RUN: close: logstash-beta-2018.09.12 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,456 INFO      DRY-RUN: close: logstash-beta-2018.09.13 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,457 INFO      DRY-RUN: close: logstash-beta-2018.09.14 with arguments: {'delete_aliases': False}
2018-09-19 16:26:46,457 INFO      Action ID: 1, "close" completed.
2018-09-19 16:26:46,457 INFO      Job completed.
```

More examples can be found here: https://github.com/elastic/curator/tree/master/examples

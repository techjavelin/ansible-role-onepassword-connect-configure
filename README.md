Ansible Role: 1Password Connect Configure
=========

This is an ansible role for configuring ansible hosts with facts during a playbook run set from a 1password vault using the 1Password Connect Server. It accepts a json file that matches this format

```
{
  "secrets": {
    "env": [
      {
        "item_name": "<<name of item in the vault>>",
        "field_name": "<<name of the field containing the secret>>",
        "fact_name": "<<name of the fact to set on the host>>"
      },
      // ...
    ],
    "env2": [
      //...
    ]
  }
}
```

Using this file it will set a fact on the target host for each secret retrieved that can be used in plays for the rest of the run, meaning only a single lookup across all plays and playbooks in a single run (including roles).

Requirements
------------

To use this role you'll need access to a 1Password Connect Server - for development environments I've included a [Makefile](examples/dev/Makefile) under the examples directory which will help you build a local development environment. In CI or production environments it's more likely you'll be connecting to a external connect server, checkout the [CI Example](examples/ci/README.md) and [Production Example](examples/production/README.md) for some ideas.

At a minimum - to run locally, you'll need
* 1Password Family, Team, or Enterprise Account
* 1Password CLI Installed on the Ansible Runner
* Docker-Compose to run the Connect Server and Sync Server

Role Variables
--------------

| Variable                    | Default              | Comments (type)                                                                                                                      |
| :-------------------------- | :------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| `op_secrets_connect_server` | localhost            | The hostname where the 1Password Connect Server is running
| `op_secrets_connect_port`   | 8080                 | The port where the 1Password Connect Server is listening 
| `op_secrets_connect_token`  | -                    | The JWT generated by the `op connect token create` command (Can also be set with the `OP_CONNECT_TOKEN` environment variable
| `op_secrets_debug`          | false                | Print out helpful debugging log messages
| `op_secrets_delegate`       | localhost            | The inventory hostname of the host where the connect server should be queried FROM                                                   |
| `op_secrets_environment`    | dev                  | The environment key to use from the `op_secrets_json_file`                                                                           |
| `op_secrets_json_file`      | -                    | Path on the runner containing the json payload for which secrets to set                                                              |
| `op_secrets_secure_logging` | true                 | Enabled secure logging to eliminate secrets being output to runner logs                                                              |
| `op_secrets_vault-uuid`     | -                    | The UUID of the vault to retrieve the secrets from                                                                                   |


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: techjavelin/op_secrets, op_secrets_json_file: "${ module.base_path }}/res/op_secrets.json", op_secrets_environment: "dev", op_secrets_vault: "some-uuid" }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

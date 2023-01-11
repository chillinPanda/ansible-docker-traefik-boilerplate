# About the project

This is a boilerplate project to kickstart your ansible, docker, traefik setup.

# Run all playbooks

```shell
ansible-playbook all.yml
```

# Configure your inventory

Copy the example and adjust to your needs / add servers.

```shell
cp inventory.example.yml inventory.yml
```

## Variable explanations

| Variable                     | Explanation                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ansible_user                 | username used by ssh to connect to the server                                                                                                                                    |
| ansible_ssh_private_key_file | path to private key file. if your ssh config already has this configured, you can omit this                                                                                      |
| host_name                    | domain of your server, e.g. used with traefik dashboard `traefik.your-server.com`                                                                                                |
| ubuntu_dist_codename         | Ubuntu distro code name. E.g. Ubuntu 22.04 is Jammy Jellyfish -> `jammy`. Use `lsb_release -cs` on your server to get the info. This is used when setting up the docker apt repo |
| ubuntu_arch                  | the server's architecture, e.g. `amd64` or `arm64`                                                                                                                               |
| traefik_basic_auth           | basic auth credentials for the traefik dashboard at `traefik.your-server.com`                                                                                                    |
| traefik_ssl_email            | email address for the let's encrypt SSL cert                                                                                                                                     |

### Authelia

All keys below are in the scope of the `authelia` object.

| Variable                | Explanation                                                                                                                                                                                                                                                                          |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| jwt_secret              | JWT used by authelia. Generate it with `docker run authelia/authelia:latest authelia crypto rand --length 64 --charset alphanumeric`, see also [official documentation](https://www.authelia.com/reference/guides/generating-secure-values/#generating-a-random-alphanumeric-string) |
| default_redirection_url | if authelia does not know where to redirect the user after login, this is used                                                                                                                                                                                                       |
| totp.issuer             | this is shown in the one time password app as the issuer. Optional, fallbacks to `host_name`                                                                                                                                                                                         |
| session.secret          | generate it like `jwt_secret`                                                                                                                                                                                                                                                        |
| session.domain          | should match whatever your root protected domain is / where authelia runs on. Optional, fallbacks to `host_name`                                                                                                                                                                     |
| storage.encryption_key  | generate it like `jwt_secret`                                                                                                                                                                                                                                                        |
| access_control.rules    | optional, further access control rules for other services you want to define                                                                                                                                                                                                         |
| notifier.smtp.username  | your SMTP's user                                                                                                                                                                                                                                                                     |
| notifier.smtp.password  | your SMTP's password                                                                                                                                                                                                                                                                 |
| notifier.smtp.host      | your SMTP's host                                                                                                                                                                                                                                                                     |
| notifier.smtp.port      | your SMTP's port                                                                                                                                                                                                                                                                     |
| notifier.smtp.sender    | the email's sender                                                                                                                                                                                                                                                                   |
| users                   | array with authelia users, structure resembles the one from authelia's config. Hash passwords with `docker run authelia/authelia:latest authelia hash-password 'yourpassword'`                                                                                                       |

# Authelia

[Authelia](https://www.authelia.com/) is used to protect services with a login. Two factor authentication is supported.

You need to configure a SMTP server for sending mails. If you don't have one, you could change the `notifier` setting in the `configuration.yml` to write to a file instead of sending mails. This should only be considered a temporary workaround. See [filesystem](https://www.authelia.com/configuration/notifications/introduction/#filesystem) setting in the authelia docs.

## Caveats

### Password reset

Users can reset their passwords. The new passwords are set in the `users_database.yml`. If you apply the playbook again, the file will be overridden and the passwords are reset to what is defined here.
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

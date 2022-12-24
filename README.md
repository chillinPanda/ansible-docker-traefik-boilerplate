# Run whole playbook

```shell
ansible-playbook oracle.yml
```

# Modifications

* Depending on your target's OS version and arch type modify the `apt_repository.repo` in roles/docker/tasks/main.yml
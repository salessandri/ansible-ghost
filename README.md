# ghost


This role sets up a [ghost blog](https://ghost.org/) using the docker community's [ghost image](https://hub.docker.com/_/ghost).

## Requirements

This role relies on `docker` being available on the host and the [`docker_container` ansible module](https://docs.ansible.com/ansible/latest/modules/docker_container_module.html) in ansible.

To cover the first one, the [`geerlingguy.docker` role](https://galaxy.ansible.com/geerlingguy/docker) can be used.

To cover the dependencies for the `docker_container` module the [`geerlingguy.pip` role](https://galaxy.ansible.com/geerlingguy/pip) can be used to install Python's [`docker` package](https://pypi.org/project/docker/).

## Role Variables

 - **`ghost__host`** (**required**, default: _localhost_): The value of this variable has 2 main uses: it defines the URL for the blog and is also used to define the container's name.
 The URL will be defined as `https://{{ ghost__host }}` while the container's name is generated by converting the variable's dots to underscores and prepending `ghost_` to it.
 - **`ghost__version`** (optional, default: _4.44.0_): ghost's docker image tag to use in the container. See [ghost image tags in dockerhub](https://hub.docker.com/_/ghost?tab=tags).
 - **`ghost__base_dir`** (optional, default: _/var/ghost-blog_): folder where to setup the blog's persistent files.
 - **`ghost__blog_name`** (optional, default: _Ghost Blog_): Name to reference a particular instance of the role within Ansible. Doesn't have any semantic effect on the service.
 - **`ghost__listen_host`** (optional, default: _127.0.0.1_): Address where the container will publish the blog's socket.
 - **`ghost__listen_port`** (optional, default: _2368_): Port where the container will publish the blog's port.
 - **`ghost__configs`** (optional, default: _{}_): Dictionary of configurations to be passed to the container as environmental variables. See [ghost's Configuration Docs](https://ghost.org/docs/concepts/config/) for all the possible values and specifically [here](https://ghost.org/docs/concepts/config/#running-ghost-with-config-env-variables) on how to convert the keys into environmental variables.

## Suggestions

It is suggested that you don't expose the ghost's port directly to the outside world but rather to use a reverse proxy such as nginx to forward the appropriate traffic.

This allows for easier TLS setup and sharing of the HTTPS port among multiple applications.

## Example Playbook

The following would be a fairly common role usage example:

```yaml
- host: my-blog.my-domain.com
  roles:
    - role: salessandri.ghost
      vars:
        ghost__host: my-blog.my-domain.com
        ghost__base_dir: /var/my-blog
        ghost__blog_name: Personal Blog
        ghost__configs:
          mail__from: '"My Blog Email" <blog@my-domain.com>'
          mail__transport: 'SMTP'
          mail__options__host: smtp.my-domain.com
          mail__options__port: 465
          mail__options__secureConnection: true
          mail__options__auth__user: blog_mail_user
          mail__options__auth__pass: '{{ blog_mail_password_vault }}'
```

## License

MIT

## Author Information

This role was created in 2020 by [Santiago Alessandri](https://rambling-ideas.salessandri.name).

# CLI Tips and Tricks
---------------------

This is a list of tips that can make you more productive on your daily work.

## Using SSH keys inside the container

You can either configure [SSH agent forwarding](https://developer.github.com/guides/using-ssh-agent-forwarding/)
with:

```yaml
volumes:
  - '{{env "SSH_AUTH_SOCK"}}:/tmp/ssh-auth-sock'
environment:
  SSH_AUTH_SOCK: "/tmp/ssh-auth-sock"
```

Or you can just share your SSH keys with the container using:

```yaml
volumes:
  - '{{env "HOME"}}/.ssh:/home/devstep/.ssh'
```

## Making project's dependencies cache persist between host restarts

Since Devstep's cache is kept at `/tmp/devstep/cache` on the host by default,
it is likely that the OS will have it cleaned when it gets restarted. In order
to make it persistent, just set it to a folder that doesn't have that behavior
(like some dir under your `$HOME`).

For example, you can add the line below to your `$HOME/devstep.yml` to configure
cached packages to be kept on `$HOME/devstep/cache`:

```yaml
cache_dir: '{{env "HOME"}}/devstep/cache'
```

## Reuse Git configurations from inside containers

```yaml
volumes:
  - '{{env "HOME"}}/.gitconfig:/home/devstep/.gitconfig'
```

## Sharing RubyGems credentials with containers

If you are a RubyGem author, you will want to publish the gem to https://rubygems.org
at some point. To avoid logging in all the time when you need to do that just
share an existing credentials file with the containers using:

```yaml
volumes:
  - '{{env "HOME"}}/.gem/credentials:/home/devstep/.gem/credentials'
```

## Sharing Heroku credentials with containers

If you deploy apps to Heroku, you will need to eventually use the Heroku Client
to interact with it. To avoid logging in all the time when you need to do that
just share the credentials file with the containers using:

```yaml
volumes:
  - '{{env "HOME"}}/.netrc:/home/devstep/.netrc'
```

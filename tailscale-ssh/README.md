# tailscale-ssh

Composite action: join a Tailscale tailnet, SSH to a node, and optionally sync files or run a command

## Usage

```yaml
- uses: secolarelabs/actions/tailscale-ssh@v0.2.0
  with:
    tailscale-oauth-client-id: ${{ secrets.TAILSCALE_OAUTH_CLIENT_ID }}
    tailscale-oauth-secret: ${{ secrets.TAILSCALE_OAUTH_SECRET }}
    tailscale-tags: tag:github
    ssh-host: my-node
    ssh-user: deploy
    ssh-private-key: ${{ secrets.REMOTE_SSH_PRIVATE_KEY }}
    app-name: demo
    env: |
      MY_KEY=my_value
    command: source ./.env && echo "$MY_KEY"
```

With no `pre-command`, `files`, `env`, or `command`, the action just joins the tailnet, opens SSH, and runs the default `echo "OK"` — a connectivity check

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `tailscale-oauth-client-id` | yes | — | Tailscale OAuth client ID |
| `tailscale-oauth-secret` | yes | — | Tailscale OAuth client secret |
| `tailscale-tags` | yes | — | ACL tags for the CI node |
| `ssh-host` | yes | — | MagicDNS name / Tailscale IP |
| `ssh-user` | yes | — | SSH user |
| `ssh-private-key` | yes | — | SSH private key |
| `app-name` | no | — | Remote dir; required if `files`/`env` set |
| `pre-command` | no | — | Command run on the node before `files` are synced (in app dir if `app-name` set) |
| `files` | no | — | Newline list of files/globs to copy |
| `env` | no | — | Multiline blob: remote `.env` (umask 077) |
| `command` | no | `echo "OK"` | Command to run (in app dir if `app-name` set) |

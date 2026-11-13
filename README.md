# ns8-pastevault

Module id: `pastevault`. Images are pinned in `build-images.sh` and updated through reviewed changes.

## Install

```bash
add-module ghcr.io/geniusdynamics/pastevault:latest 1
```

The command returns the instance id, for example `pastevault1`. Use the id NS8 returns.

## Configure

```bash
api-cli run configure-module --agent module/pastevault1 --data - <<'EOF'
{
  "host": "pastevault.example.org",
  "http2https": true,
  "lets_encrypt": false
}
EOF
```

| Parameter | Required | Type | Description |
|---|---|---|---|
| `host` | yes | string | Host name for the application, like 'pastevault.domain.org' |
| `lets_encrypt` | yes | boolean | Request a valid Let's Encrypt certificate. |
| `http2https` | yes | boolean | Redirect all the HTTP requests to HTTPS |

## Configuration output

`get-configuration` returns: `host`, `lets_encrypt`, `http2https`.

## Images

| Image | Version |
|---|---|
| `docker.io/postgres` | `18.7` |
| `ghcr.io/arc53/pastevault-backend` | `v0.1.5` |
| `ghcr.io/arc53/pastevault-frontend` | `v0.1.5` |

## Network and access

- Reserves 2 TCP port(s) on the node. The web application is published through Traefik on the configured host name.
- Runs rootless.
- Declares these NS8 authorizations: `traefik@node:routeadm`.

## Backup

The following paths are included in the NS8 backup (`imageroot/etc/state-include.conf`):

- `state/frontend.env`
- `state/postgres.env`
- `volumes/postgresdata`
- `state/backend.env`

## Services

- `backend-app.service`
- `frontend-app.service`
- `pastevault.service`
- `postgres-app.service`

## Get the configuration

```bash
api-cli run get-configuration --agent module/pastevault1
```

## Update and uninstall

```bash
api-cli run update-module --data '{"module_url":"ghcr.io/geniusdynamics/pastevault:latest","instances":["pastevault1"],"force":true}'
remove-module --no-preserve pastevault1
```

## Testing

Continuous integration:

- `Publish images` builds the module image on every push.
- `Test module` installs the image on a QEMU NS8 node and runs the Robot Framework suite in `tests/`. It runs after `Publish images` succeeds on `dev` and on `main`.
- `Build API JSON Schema docs` and the security scan run alongside.

The suite installs the module, configures a host name, checks that the web application answers through Traefik, takes UI screenshots and removes the module.

Run it locally with the NS8 standard infrastructure described in the [ns8-github-actions README](https://github.com/NethServer/ns8-github-actions/blob/v1/README.md#running-tests-locally).

## Open items

Found by the NS8 module standards check. These must be resolved before a stable release:

- The module description is still the template text: write a description of at least 60 characters in ui/public/metadata.json
- tests/pastevault.robot generated from template: set real configure payload and URL check
- metadata: documentation_url is a placeholder
- metadata: categories empty (use standard categories)
- metadata: description.en needs >= 60 real chars (+ it translation)
- backend-app.service: uses %S/state, kickstart uses %E/state
- frontend-app.service: uses %S/state, kickstart uses %E/state
- pastevault.service: uses %S/state, kickstart uses %E/state
- postgres-app.service: uses %S/state, kickstart uses %E/state
- discover-smarthost still present: remove if the app does not use SMTP
- backend-app.service: %S/state changed to %E/state (review the bind mounts that remain)
- frontend-app.service: %S/state changed to %E/state (review the bind mounts that remain)
- pastevault.service: %S/state changed to %E/state (review the bind mounts that remain)
- postgres-app.service: %S/state changed to %E/state (review the bind mounts that remain)
- imageroot/actions/restore-module/80start_services: smarthost lines removed, check the unit still starts
- imageroot/actions/configure-module/80start_services: smarthost lines removed, check the unit still starts
- imageroot/systemd/user/backend-app.service: smarthost lines removed, check the unit still starts
- imageroot/systemd/user/frontend-app.service: smarthost lines removed, check the unit still starts
- imageroot/systemd/user/postgres-app.service: smarthost lines removed, check the unit still starts
- PASTEVAULT_BACKEND_IMAGE is used by the module but no image in the label provides it: leftover from the template?
- PASTEVAULT_FRONTEND_IMAGE is used by the module but no image in the label provides it: leftover from the template?
- POSTGRES_IMAGE is used by the module but no image in the label provides it: leftover from the template?

## Upstream

Application documentation: https://docs.pastevault.com/

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).

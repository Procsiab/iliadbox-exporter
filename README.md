# Credits: [trazfr/freebox-exporter](https://github.com/trazfr/freebox-exporter)

This code is the quickest possible way to adapt the great work from user [trazfr](https://github.com/trazfr) to export Prometheus metrics for an Italian Iliadbox (basically a rebrand of the Freebox) only through API calls over the LAN.

## ⚠️  Warning 🩹

To make HTTPS calls over the LAN and reuse as much code as possible, I am actually using the TLS client option `InsecureSkipVerify`, which is relatively **not good**. That being said, you decide if for you it's good enough, or if you know a better solution let me know - even if it's not that "easy" to implement.

# Iliadbox Exporter

This code will realize a Prometheus compatible metrics exporter, which collects data from the Iliadbox router.

## Build

*Optional*: To build using a Golang `1.20` container, run the following commands (assuming Podman is installed):

```bash
podman run --rm -it -v $(pwd):/repo:Z docker.io/amd64/golang:1.20.6-alpine sh
cd /repo
```

For building the binary, run `go build` from the repository's folder, or use `get` directly as follows:

```bash
go install github.com/Procsiab/iliadbox-exporter@latest
```

## Usage

The following options are available:

- `debug`: More verbose log output
- `hostDetails`: Collect connected hosts details
- `httpDiscovery`: Do not rely on mDNS for discovering the Iliadbox's address
- `listen`: Choose a port to bind to (defaults to 9091)

After passing the options, it is **mandatory** to pass a path to the JSON file that will store the authentication token.

An example command which will start the exporter is the following:

```bash
iliadbox-exporter -hostDetails -httpDiscovery -listen ":9091" -debug auth_token.json
```

### API Authorization

If it's the first time adding an app to your Iliadbox, the procedure is briefly reported below:

- Run the exporter, which will start a challenge against the Iliadbox
- Click the right arrow on the Iliadbox to authorize the request (you will also see the application ID on the display)
- Go to your Iliadbox settings page and under the Access Management, on the Applications tab, edit the permissions for the "iliadbox-exporter" and select at least "Manage settings"
- Restart the exporter, passing the same path for the token file

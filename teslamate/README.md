# TeslaMate Addon for Home Assistant

This addon builds on the excellent work of [Adrian Kumpf](https://github.com/adriankumpf/teslamate). See his repo for information regarding the TeslaMate application.

## DB Connection

If using the Postgres addon from this repo, the database host is ```29b65938-postgres```

## No Ingress Support

TeslaMate does not currently support a base path, so it expects all requests to be generated from the URL root ```/```.
See https://github.com/adriankumpf/teslamate/issues/494.

This means we cannot implement Ingress and must expose TeslaMate to the network.
I recommend you only do this to configure TeslaMate and you then remove the external port mapping.

## Uploading Grafana Dashboards

*I recommend you use the existing Grafana addon from the community addons*

This must be done manually for now. You must use a bash terminal and have ```curl``` available.

- Use the Grafana addon from the community repository.

- Add the following plugins to the Grafana addon configuration:

```yaml
plugins:
  - pr0ps-trackmap-panel
  - natel-discrete-panel
  - grafana-piechart-panel
```

---

## ⚠️ Old Broken Instructions

These instructions don't work because Grafana won't accept incoming connects - it either redirects to the HTML page or returns a 401 Access denied error. See later for my workaround.

- Expose the Grafana UI on an external port and restart it

- Making  note of the admin username and password (see the addon container logs), clone the [teslamate repo](https://github.com/adriankumpf/teslamate)

```bash
git clone https://github.com/adriankumpf/teslamate.git
```

- Run the dashboards script to create the necessary dashboards:

```bash
URL="http://my-homeassistant-host:3000" \
LOGIN="admin:mysecretpassword" \
DASHBOARDS_DIRECTORY="./dashboards" \
./dashboards.sh restore
```

- Finally remove the port mapping from Grafana and restart the addon

---

## New Fudge Instructions

- Enable root access to your Home Assistant host
- Connect over SSH and run `login` to get a prompt
- Find the Grafana container: `docker container ls`
- Connect to the containter: `docker exec -it <container_id> bash`
- Update apt: `apt-get update`
- Install git: `cd /tmp && apt-get install git`
- Clone the Teslamate repo: `git clone https://github.com/adriankumpf/teslamate.git`
  - You can also clone just the grafana directory using the new experimental partial clone functionality recently enabled on GitHub.com (Apr 2020) if you have git 2.26.0 or later:

  ```bash
  $ git clone --no-checkout --filter=blob:none https://github.com/adriankumpf/teslamate.git
  $ cd teslamate
  $ git sparse-checkout init
  $ git sparse-checkout set grafana
  ```

- Run the dashboards script to create the necessary dashboards:

```bash
$ cd grafana
$ URL="http://localhost:3000" \
  LOGIN="admin:hassio" \
  DASHBOARDS_DIRECTORY="./dashboards" \
  ./dashboards.sh restore
```

- Remove the git repo: `rm -rf /tmp/teslamate`
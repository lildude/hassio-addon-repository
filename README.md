# My Addon Repo

Some of my addons for Home Assistant.

* [Postgres](postgres)
* [TeslaMate](teslamate) - **Armv7 ONLY**

## CI/CD

I use GitHub Actions for CI/CD, see the [workflows](.github/workflows) directory.

## Adding to Home Assistant

Use the following URL: ```https://github.com/matt-FFFFFF/hassio-addon-repository```

---

@lildude's changes:

- Updated the documentation for uploading the Grafana dashboards as the latest Grafana addon doesn't seem to play nice when attempting to use the API:
  - accessing directly redirects to the HTML login page
  - accessing via the ingress URL always results in a `401: unauthorised` error
  I suspect this is due to a config error in the nginx config, but I've not dug into it yet.
- Added a `build.json` file to the `teslamate` extension so I can use <https://github.com/home-assistant/hassio-builder> to build the multi-arch images:
  ```
  docker run --rm --privileged -v /path_to/teslamate:/data homeassistant/amd64-builder --all -t /data -v dev --docker-user <me> --docker-password <its_a_secret_>
  ```
  Why? Because the matt-FFFFFF armv7 image contains binaries built on amd64 so running teslamate fails with the error `bin/erlexec ELF: not found` error when run on a Raspberry Pi.
  **Update:** This breaks for me too 😭.
- **Update 2:** I've now pushed the armv7 image built on my Raspberry Pi under HomeAssistant and put back the image line.
- A few other tweaks to point to this repo or my Docker Hub repo.
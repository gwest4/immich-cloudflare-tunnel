# Immich with Remote Access via Cloudflare Tunnel

This repository contains the default [`docker-compose.yml`](https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml) and [`example.env`](https://github.com/immich-app/immich/releases/latest/download/example.env) provided by the Immich [Docker Compose installation instructions](https://docs.immich.app/install/docker-compose), but with added configuration to make the server accessible on the internet via Cloudlfare Tunnel.

The Cloudflare Tunnel setup steps are modified from [Cloudflare's guide](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/).

## Setup Instructions

1. Rename `example.env` to `.env` and populate it with the appropriate values to configure Immich. [(guide)](https://docs.immich.app/install/docker-compose/#step-2---populate-the-env-file-with-custom-values).

1. Create a remotely-managed Cloudflare tunnel and publish an application route to Immich.

    1. Log in to [Zero Trust](https://one.dash.cloudflare.com/) and go to **Networks > Tunnels**.

    1. Select **Create a tunnel**.

    1. Choose **Cloudflared** for the connector type and select **Next**.

    1. Enter a name for your tunnel. We suggest choosing a name that reflects the type of resources you want to connect through this tunnel (for example, `enterprise-VPC-01`).

    1. Select **Save tunnel**.

    1. Under **Choose an environment**, select **Docker**.

    1. Under **Install and run a connector**, copy the `--token` value from the provided `docker run` command and set it as the `TUNNEL_TOKEN` in your `.env`.

    1. Go to the **Published application routes** tab and select **Add a published application route**.

    1. Enter a subdomain and select a **Domain** from the dropdown menu. Specify any subdomain or path information.
        > Note: If you add a multi-level subdomain (more than one level of subdomain), you must [order an Advanced Certificate for the hostname](https://developers.cloudflare.com/cloudflare-one/faq/troubleshooting/#i-see-this-site-cant-provide-a-secure-connection).

    1. Under **Service**, choose the **HTTP** service type and specify `immich-server:2283` as its URL.
        > This tells the `cloudflared` daemon to send traffic to the Immich server container.

    1. Select **Complete setup**.

1. Run `docker-compose up`.
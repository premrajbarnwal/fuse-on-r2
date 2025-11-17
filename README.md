# Cloudflare Containers + R2-backed FUSE mounts

This is a demo app that 

1. A Worker as the front-end that proxies to a single container instance
2. A container with an R2 bucket mounted using [tigrisfs](https://github.com/tigrisdata/tigrisfs) at `$HOME/mnt/r2/<bucket_name`>
3. A Go application that uses `io/fs` to list files in the mounted R2 bucket and return them as JSON

## Deploying it

You'll need to provide your [R2 API credentials](https://developers.cloudflare.com/r2/api/tokens/) and Cloudflare account ID to the container.

1. Update `wrangler.jsonc` with the `BUCKET_NAME` and `ACCOUNT_ID` environment variables. These are OK to be public.
2. Use `npx wrangler@latest secret put AWS_ACCESS_KEY_ID` and `npx wrangler@latest secret put AWS_SECRET_ACCESS_KEY` to set your R2 credentials.
3. Ensure Docker is running locally.
4. `npx wrangler@latest deploy`

You can mount multiple buckets as you wish by updating the Dockerfile or doing it dynamically from within the application in your container. `tigrisfs` does not currently support scoping a mount to a specific prefix.

## Learn More

To learn more about Containers, take a look at the following resources:

- [Container Documentation](https://developers.cloudflare.com/containers/) - learn about Containers
- [Container Class](https://github.com/cloudflare/containers) - learn about the Container helper class

## License

Apache-2.0 licensed. Copyright 2025, Cloudflare, Inc.

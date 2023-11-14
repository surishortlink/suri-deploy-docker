# Suri × Deploy with Docker

You're viewing a template repository tailored for deploying Suri with Docker.
That could be on a cloud platform that supports Docker container images, or just
on your own machine.

What's Suri? Suri is your own link shortener that's easily deployed as a static
site. No server-side hosting, serverless cloud functions, or database necessary.
Head over to the main [`jstayton/suri`](https://github.com/jstayton/suri)
repository to learn more, including additional deployment methods.

## Setup

1. Click the "Use this template" button above and then "Create a new
   repository". Fill in the required details to create a new repository based on
   this one.
2. Make sure you have Docker installed and running.
   [Docker Desktop](https://www.docker.com/products/docker-desktop) is an easy
   way to get started on your own machine.
3. Build a Docker image named `suri` from the [`Dockerfile`](Dockerfile):

   ```bash
   docker build --tag suri .
   ```

   This uses
   [docker-static-website](https://github.com/lipanski/docker-static-website) to
   produce a very small image (~154 KB plus the size of the static site).

4. Start a new container using the image, which serves the static site on port
   `3000`:

   ```bash
   docker run --interactive --tty --rm --publish 3000:3000 suri
   ```

If you're just looking to build the static site, but not serve it, you can
change the `docker build` command to export the `build` directory from the
Docker image to your host machine:

```bash
docker build --target=export --output=build --tag suri .
```

Keep in mind that this won't remove any existing files in the `build` directory
on your host machine, so you may want to delete the directory beforehand.

## Manage Links

Links are managed through [`./src/links.json`](./src/links.json), which is
seeded with a few examples to start:

```json
{
  "/": "https://github.com/jstayton/suri",
  "1": "https://fee.org/articles/the-use-of-knowledge-in-society/",
  "tw": "https://twitter.com"
}
```

It couldn't be simpler: the key is the "shortlink" path that gets redirected,
and the value is the target URL. Keys can be as short or as long as you want,
using whatever mixture of characters you want. `/` is a special entry for
redirecting the root path.

## Config

Config options are set in [`suri.config.json`](suri.config.json). There is only
one at this point:

| Option | Description                                                        | Type    | Default |
| ------ | ------------------------------------------------------------------ | ------- | ------- |
| `js`   | Whether to redirect with JavaScript instead of a `<meta>` refresh. | Boolean | `false` |

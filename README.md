# errorgap-setup-action

Exports an Errorgap project's connection to the rest of the job, so later
steps (a Docker build, an env file copied to a server, the
[sourcemaps](https://github.com/errorgaphq/errorgap-sourcemaps-action) and
[deploy](https://github.com/errorgaphq/errorgap-deploy-action) actions) can use
it without repeating the values.

Without an API key it exports nothing and reporting stays off, so one workflow
serves environments that report and environments that don't.

## Usage

```yaml
- uses: errorgaphq/errorgap-setup-action@v1
  with:
    api-key: ${{ secrets.ERRORGAP_API_KEY }}
    endpoint: https://acme.errorgap.com
    project-slug: my-project
    environment: production
```

With `nuxt: "true"` it also exports the `NUXT_PUBLIC_ERRORGAP_*` variables
that [`@errorgap/nuxt`](https://github.com/errorgaphq/errorgap-nuxt) reads
through Nuxt's runtime config.

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `api-key` | no | | The project's API key (a secret). Leave it empty to export nothing. |
| `endpoint` | yes | | The Errorgap instance, such as `https://acme.errorgap.com`. |
| `project-slug` | yes | | The project's slug in Errorgap. |
| `environment` | yes | | The environment reported with errors (`production`, `staging`, `beta`, ...). |
| `release` | no | `github.sha` | The release reported with errors. |
| `nuxt` | no | `false` | Also export `NUXT_PUBLIC_ERRORGAP_*` for `@errorgap/nuxt`. |

## Outputs

| Output | Description |
|---|---|
| `enabled` | `true` when the variables were exported, `false` when there was no API key. |

## What it exports

With an API key:

```
ERRORGAP_API_KEY
ERRORGAP_ENDPOINT        (only when not already set to the same value)
ERRORGAP_PROJECT_SLUG    (only when not already set to the same value)
ERRORGAP_ENVIRONMENT
ERRORGAP_RELEASE
```

And with `nuxt: "true"`:

```
NUXT_PUBLIC_ERRORGAP_API_KEY
NUXT_PUBLIC_ERRORGAP_ENDPOINT
NUXT_PUBLIC_ERRORGAP_PROJECT_SLUG
NUXT_PUBLIC_ERRORGAP_ENVIRONMENT
NUXT_PUBLIC_ERRORGAP_RELEASE
```

The API key is masked in the job log.

## License

MIT.

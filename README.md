# Coolify Deploy GitHub Action

A GitHub Action to trigger deployments on [Coolify](https://coolify.io/) via its API.

## Features

- Deploy by resource UUID or tag
- Automatic pull request detection (uses PR number for PR-specific deployments)
- Force rebuild option (without cache)
- Returns deployment UUIDs for tracking

## Usage

### Basic Usage (Deploy by UUID)

```yaml
- name: Deploy to Coolify
  uses: AmenityDev/coolify-action@v1
  with:
    coolify_url: https://coolify.example.com
    token: ${{ secrets.COOLIFY_TOKEN }}
    uuid: 'your-resource-uuid'
```

### Deploy by Tag

```yaml
- name: Deploy to Coolify
  uses: AmenityDev/coolify-action@v1
  with:
    coolify_url: https://coolify.example.com
    token: ${{ secrets.COOLIFY_TOKEN }}
    tag: 'production'
```

### Deploy Multiple Resources

```yaml
- name: Deploy to Coolify
  uses: AmenityDev/coolify-action@v1
  with:
    coolify_url: https://coolify.example.com
    token: ${{ secrets.COOLIFY_TOKEN }}
    uuid: 'uuid-1,uuid-2,uuid-3'
```

### Force Rebuild (No Cache)

```yaml
- name: Deploy to Coolify (Force)
  uses: AmenityDev/coolify-action@v1
  with:
    coolify_url: https://coolify.example.com
    token: ${{ secrets.COOLIFY_TOKEN }}
    uuid: 'your-resource-uuid'
    force: 'true'
```

### Pull Request Deployments

When used in a `pull_request` event, the action automatically includes the PR number in the API request:

```yaml
name: Deploy PR Preview

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy PR Preview
        uses: AmenityDev/coolify-action@v1
        with:
          coolify_url: https://coolify.example.com
          token: ${{ secrets.COOLIFY_TOKEN }}
          uuid: 'your-resource-uuid'
          # PR number is automatically detected and sent to Coolify
```

> **Note:** The `tag` parameter cannot be used with pull request events.

## Inputs

| Input         | Description                                             | Required | Default |
| ------------- | ------------------------------------------------------- | -------- | ------- |
| `coolify_url` | The base URL of your Coolify instance                   | Yes      | -       |
| `token`       | Coolify API token for authentication                    | Yes      | -       |
| `uuid`        | Resource UUID(s). Comma separated list is also accepted | No*      | -       |
| `tag`         | Tag name(s). Comma separated list is also accepted      | No*      | -       |
| `force`       | Force rebuild without cache                             | No       | `false` |

\* Either `uuid` or `tag` must be provided.

## Outputs

| Output        | Description                                     |
| ------------- | ----------------------------------------------- |
| `deployments` | JSON response containing deployment information |

### Example Output

```json
{
  "deployments": [
    {
      "message": "Deployment started",
      "resource_uuid": "abc123",
      "deployment_uuid": "deploy-xyz"
    }
  ]
}
```

## Setting Up Your Coolify Token

1. Go to your Coolify instance
2. Navigate to **Security** → **API Tokens**
3. Create a new token with appropriate permissions
4. Add the token to your GitHub repository secrets as `COOLIFY_TOKEN`

## License

MIT

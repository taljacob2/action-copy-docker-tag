![Build](https://github.com/julb/action-copy-docker-tag/workflows/Build/badge.svg)

# GitHub Action to copy a Docker tag and push to any Docker registry

The GitHub Action to create Docker tags from an existing one and push to a Docker registry server (the default is DockerHub).

It expects the following secrets:

- `DOCKER_LOGIN_USERNAME` : the Docker registry login username.
- `DOCKER_LOGIN_PASSWORD` : the Docker registry login password.
- `DOCKER_LOGIN_SERVER` : the Docker registry login server (the default is DockerHub).

## Usage

### Example Workflow file

- Copy and Push Docker tags (inline):

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Copy and Push Docker tags
        uses: julb/action-copy-docker-tag@v1
        with:
          username: ${{ secrets.DOCKER_LOGIN_USERNAME }}
          password: ${{ secrets.DOCKER_LOGIN_PASSWORD }}
          server: ${{ secrets.DOCKER_LOGIN_SERVER }}
          from: julb/some-image:version
          tags: |
            julb/some-image:new-version-1
            julb/some-image:new-version-2
```

- Copy and Push Docker tags (from file):

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Copy and Push Docker tags
        uses: julb/action-copy-docker-tag@v1
        with:
          username: ${{ secrets.DOCKER_LOGIN_USERNAME }}
          password: ${{ secrets.DOCKER_LOGIN_PASSWORD }}
          server: ${{ secrets.DOCKER_LOGIN_SERVER }}
          from_file: [some-file].[json|txt|properties]
```

where `[some-file]` is:

- Either a JSON file (filename should end with `.json`):

```json
{
  "julb/some-image:version": [
    "julb/some-image:new-version-1",
    "julb/some-image:new-version-2"
  ],
  "julb/some-image-2:version": [
    "julb/some-image-2:new-version-1",
    "julb/some-image-2:new-version-2"
  ]
}
```

The keys of the dictionary are expected to be the source images.
The values of the dictionary are expected to be the tags to create for the respective source images.

- Either a text file (filename should end with `.txt` or `.properties`):

```txt
julb/some-image:version=julb/some-image:new-version-1,julb/some-image:new-version-2
julb/some-image-2:version=julb/some-image-2:new-version-1,julb/some-image-2:new-version-2
```

Other file extensions are not supported.

### Inputs

| Name        | Type     | Default     | Description                                                                                                                  |
| ----------- | -------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `username`  | string   | `Not set`   | The Docker registry login username. **Required**                                                                             |
| `password`  | string   | `Not Set`   | The Docker registry login password. **Required**                                                                             |
| `server`    | string   | `docker.io` | The Docker registry login server. **Optional** - Default is DockerHub.                                                       |
| `from`      | string   | `Not set`   | The Docker tag to copy. **Required** if `from_file` is not set.                                                              |
| `tags`      | string[] | `Not Set`   | The Docker tags to create from `from` tag and push. **Required** if `from_file` is not set.                                  |
| `from_file` | filename | `Not set`   | A file which provides ability to configure multiple sources and destinations. **Required** if `from` and `tags` are not set. |

**Important note**: Docker login credentials contains sensitive values and should be provided using Github Action Secrets.
Don't paste your Docker login credentials in clear in your Github action.

### Outputs

| Name | Type | Description |
| ---- | ---- | ----------- |

## Contributing

This project is totally open source and contributors are welcome.

When you submit a PR, please ensure that the python code is well formatted and linted.

```
$ make install.dependencies
$ make format
$ make lint
```

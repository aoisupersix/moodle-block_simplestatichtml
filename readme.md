moodle-simple_static_html
===

A sample moodle block plugin that uses a [vscode remote containers](https://code.visualstudio.com/docs/remote/containers).
Based on the [moodle plugin development guide](https://docs.moodle.org/dev/Blocks).

Packed with:
- Docker image for moodle plugin development with php,composer,xdebug,phpcs preconfigured.
- Maria db container with persistent volumes.
- Preconfigured remote debugging with xdebug on vscode.
- Preconfigured linter and formatter by phpcs configured for moodle based on [moodle-local_codechecker](https://github.com/moodlehq/moodle-local_codechecker/blob/master/moodle/ruleset.xml).

## Requirement
- [vscode](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)
- [docker](https://www.docker.com/)

## Installation

1. Clone repository
```bash
git clone https://github.com/aoisupersix/moodle-simple_static_html
code moodle-simple_static_html
```

2. Run ``Extensions: Configure Recommended Extensions (Workspace Folder)`` from the vscode command palette

3. Run ``Remote-Containers: Open Workspace in Container...`` from the vscode command palette

## Change moodle version

Change the ``MOODLE_VERSION`` in [.devcontainer/docker-compose.yml](https://github.com/aoisupersix/moodle-simple_static_html/blob/master/.devcontainer/docker-compose.yml)

```
args:
- MOODLE_VERSION=${moodle_version}
```

## Change plugin type and plugin name

Change directory path of the following properties↓

- ``workspaceFolder`` in [.devcontainer/devcontainer.json](https://github.com/aoisupersix/moodle-simple_static_html/blob/master/.devcontainer/devcontainer.json)

```
"workspaceFolder": "/var/www/html/${plugin_type}/${plugin_root_directory}",
```

- ``volumes`` in [.devcontainer/docker-compose.yml](https://github.com/aoisupersix/moodle-simple_static_html/blob/master/.devcontainer/docker-compose.yml)

```
volumes:
    - './php.ini:/usr/local/etc/php/php.ini'
    - './container-volumes/moodledata_data:/var/www/moodledata'
    - '..:/var/www/html/${plugin_type}/${plugin_root_directory}:cached'
```

- ``pathMappings`` in [.vscode/launch.json](https://github.com/aoisupersix/moodle-simple_static_html/blob/master/.vscode/launch.json)

```
"pathMappings": {
    "/var/www/html/${plugin_type}/${plugin_root_directory}": "${workspaceFolder}"
}
```

## Clear container volumes

```
rm -rf .devcontainer/container-volumes
```

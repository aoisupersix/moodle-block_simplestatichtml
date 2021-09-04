# moodle-plugin-devcontainer

This is a sample code to create a moodle plugin development environment with [vscode remote containers](https://code.visualstudio.com/docs/remote/containers).
The plugin is a sample that adds a static HTML block. Based on [moodle plugin development guide](https://docs.moodle.org/dev/Blocks).

DevContainer packed with:

-   Docker image for moodle plugin development with php,composer,xdebug,phpcs preconfigured.
-   Maria db container with persistent volumes.
-   Preconfigured remote debugging with xdebug on vscode.
-   Preconfigured linter and formatter by phpcs configured for moodle based on [moodle-local_codechecker](https://github.com/moodlehq/moodle-local_codechecker/blob/master/moodle/ruleset.xml).
-   phpMyAdmin manipulating a moodle db.

## Requirement

-   [vscode](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)
-   [docker](https://www.docker.com/)

## Installation

1. Clone repository

```bash
$ git clone https://github.com/aoisupersix/moodle-plugin-devcontainer
$ code moodle-plugin-devcontainer/
```

2. Run `Extensions: Configure Recommended Extensions (Workspace Folder)` from the vscode command palette

3. Run `Remote-Containers: Open Workspace in Container...` from the vscode command palette

4. Open `http://localhost:8080` and do the moodle installation

## Debugging

Select `XDebug on DevContainer` from `RUN AND DEBUG` on vscode.

![](/images/1.png)

## Using phpMyadmin

Open `http://localhost:8081`

## Change moodle version

Change the `MOODLE_VERSION` in [.devcontainer/docker-compose.yml](https://github.com/aoisupersix/moodle-plugin-devcontainer/blob/master/.devcontainer/docker-compose.yml)

```
args:
- MOODLE_VERSION=${moodle_version}
```

## Change plugin type and plugin name

Change directory path of the following properties↓

-   `workspaceFolder` in [.devcontainer/devcontainer.json](https://github.com/aoisupersix/moodle-plugin-devcontainer/blob/master/.devcontainer/devcontainer.json)

```
"workspaceFolder": "/var/www/html/${plugin_type}/${plugin_name}",
```

-   `volumes` in [.devcontainer/docker-compose.yml](https://github.com/aoisupersix/moodle-plugin-devcontainer/blob/master/.devcontainer/docker-compose.yml)

```
volumes:
    - './php.ini:/usr/local/etc/php/php.ini'
    - './container-volumes/moodledata_data:/var/www/moodledata'
    - '..:/var/www/html/${plugin_type}/${plugin_name}:cached'
```

-   `pathMappings` in [.vscode/launch.json](https://github.com/aoisupersix/moodle-plugin-devcontainer/blob/master/.vscode/launch.json)

```
"pathMappings": {
    "/var/www/html/${plugin_type}/${plugin_name}": "${workspaceFolder}"
}
```

## Change moodle settings

Change [.devcontainer/moodle/config.php](https://github.com/aoisupersix/moodle-plugin-devcontainer/blob/master/.devcontainer/moodle/config.php)

## Change server timezone and locale

Change [.devcontainer/moodle/Dockerfile](https://github.com/aoisupersix/moodle-plugin-devcontainer/blob/master/.devcontainer/moodle/Dockerfile)

```
# Set timezone
ENV TZ Asia/Tokyo
RUN echo "${TZ}" > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata

# Set locale
RUN echo 'ja_JP.UTF-8 UTF-8' >> /etc/locale.gen \
    && locale-gen \
    && update-locale LANG=ja_JP.UTF-8
```

## Clear container volumes

```
rm -rf .devcontainer/container-volumes
```

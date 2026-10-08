# Updating an existing installation

## Summary

Pull the latest changes of the `development` repository and apply any new shell aliases to your `~/.bash_profile`.
Aliases such as `node24` are written to `~/.bash_profile` only when the environment is first provisioned, so an existing installation does not receive them automatically.

## General procedure

Follow these steps whenever a new release of this repository introduces changes that affect an installation you have already provisioned.

Connect to your **AlmaLinux 10** distro, then go to the directory where you cloned the repository (by default `~/development`) and pull the latest changes:

```shell
cd ~/development
git pull
```

Open the template that defines the shell aliases and compare it with your own `~/.bash_profile`:

```shell
cat wsl/roles/system/templates/bash_profile.j2
```

Open `~/.bash_profile` in an editor and add any alias that is missing from it:

```shell
nano ~/.bash_profile
```

Save the file, then log out and log back in (or run `source ~/.bash_profile`) so that the new aliases are loaded.

> Only add what is new.
> Do not replace the whole file, as it may contain your own customizations.

## Changes

### Node.js 24 by default

Node.js 24 is now the default version, and the `node24` alias is available.
To upgrade an existing installation:

1. Go to your clone of the repository and pull the latest changes:

    ```shell
    cd ~/development
    git pull
    ```

2. Open `~/.bash_profile`:

    ```shell
    nano ~/.bash_profile
    ```

3. Add the following line:

    ```shell
    alias node24="sudo dnf remove nodejs -y && curl -fsSL https://rpm.nodesource.com/setup_24.x | sudo bash - && sudo dnf install nodejs -y"
    ```

4. Save the file, then log out and log back in.
5. Run the alias to switch to Node.js 24:

    ```shell
    node24
    ```

6. Check the installed version:

    ```shell
    node -v
    ```

    The output should look similar to the below:

    ```terminaloutput
    v24.13.0
    ```

For more on switching between Node.js versions, see the [FAQ](faq.md#how-do-i-switch-to-a-different-version-of-nodejs).

# AlmaLinux 10 Create virtualhosts

## Summary

Add your desired domains to `config.yml` and run the `create-virtualhost.yml` Ansible playbook to provision each one.

> By using the `*.localhost` pattern for any new virtualhost, you do not need to modify the `hosts` file in Windows, because these are routed by default.

> This assumes you've already completed [Setup Packages](../setup/setup-packages.md).
> Apache must already be installed before virtualhosts can be created.

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

If you don't already have a `config.yml` file, duplicate `config.yml.dist` as `config.yml`.

Using your preferred text editor, open `config.yml` and, under the `virtualhosts` key, enter the virtualhosts that you want to create, each on its own line.
Already existing virtualhosts will be skipped, their contents will not be lost, no need to comment or remove them.

> Virtualhost names should be valid hostnames (lowercase letters, numbers, and hyphens) ending in `.localhost`, for example `api.dotkernel.localhost`.
> Avoid spaces or other special characters.

> You can come back and add more virtualhosts at any time: add new lines under `virtualhosts` and re-run the playbook.
> Existing virtualhosts are left untouched.

Save and close the file.

Create the specified virtualhosts:

```shell
ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass
```

This process will ask for your **AlmaLinux 10** password, iterate over the list of configured, and output a short summary with the results.
Your virtualhost should be accessible and ready to use.

You will install your project under the `html` directory of your project, for example `/var/www/example.localhost/html`.

> The virtualhost's document root is set to the `public` directory of the above location, for example `/var/www/example.localhost/html/public`.

> If you want to have the DocumentRoot directly in `html` folder, you need to modify the file `/etc/httpd/sites-available/example.localhost`.

> A freshly created virtualhost only has an empty `html` directory - `html/public` doesn't exist yet.
> Visiting the virtualhost's URL in your browser will show an error until you place a project there, this is expected.

> Apache logs for each virtualhost are stored separately, for example `/var/www/example.localhost/log/error.log` and `/var/www/example.localhost/log/requests.log`.
> Check these first if something isn't working as expected.

## Good to know

* To run your installed projects, you need to start **AlmaLinux 10** first.
* If you work with virtualhosts, your projects are created under `/var/www/`.
* You can still run PHP scripts under the default Apache project directory, located at `/var/www/html/`.
* If you encounter write permission issues, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-fix-common-permission-issues).
* We install PHP 8.4 by default-if you need a different version, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-switch-to-a-different-version-of-php).
* We install Node.js 22 by default-if you need a different version, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-switch-to-a-different-version-of-nodejs).

## FAQ

**Q: What if I don't have a `config.yml` file yet?**

A: Duplicate the provided template by running the below command inside `development/wsl`:

```shell
cp config.yml.dist config.yml
```

Then add your virtualhosts under the `virtualhosts` key.

**Q: What happens if I list a virtualhost that already exists?**

A: It will simply be skipped.
Its existing files and configuration are left untouched, so there's no need to comment out or remove already-created entries from `config.yml`.

**Q: Where should I put my project's files?**

A: Under the `html` directory of your virtualhost, for example `/var/www/example.localhost/html`.
The document root is set to the `public` subdirectory of that location, for example `/var/www/example.localhost/html/public`.

**Q: How do I make the DocumentRoot point directly to `html` instead of `html/public`?**

A: Edit the virtualhost's Apache configuration file at `/etc/httpd/sites-available/example.localhost` and change the `DocumentRoot` accordingly.

**Q: How do I access my virtualhost if AlmaLinux 10 isn't running?**

A: You need to start **AlmaLinux 10** first.
Your virtualhosts are only reachable while the distro is running.

**Q: What if I hit permission errors when writing to my project's files?**

A: See the [permission issues guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-fix-common-permission-issues) in the FAQ.

**Q: How do I delete a virtualhost I no longer need?**

A: See [How do I delete a virtualhost?](https://docs.dotkernel.org/development/v2/faq/#how-do-i-delete-a-virtualhost) in the FAQ.

# Running AlmaLinux 10 on WSL 2

## Summary

Start (or reconnect to) your AlmaLinux 10 distro from Windows Terminal, check whether it's currently running, and shut it down when you're done.

> If you are not using WSL 2, connect via SSH with [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/).

Open `Windows Terminal`.

Start **AlmaLinux 10** by executing:

```shell
wsl -d AlmaLinux-10
```

OR

Locate the app selector dropdown in `Windows Terminal`'s title bar and click `AlmaLinux-10`.
This will open a new tab connected to **AlmaLinux 10**.

> To run your applications using WSL 2, you always need to be connected to your **AlmaLinux 10** distribution.
> For this, all you need to do is to launch it from a terminal, file explorer or an IDE.
> Once launched, you can close the tool you launched it from.
> It will stay connected until you shut it down manually or reboot your computer.

## Check if AlmaLinux 10 is running

From Windows Terminal, execute:

```shell
wsl -l -v
```

Look for the `AlmaLinux-10` row — its **STATE** column reads **Running** if it's currently active, or **Stopped** otherwise.

## Shut down AlmaLinux 10

To stop just the AlmaLinux 10 distro, leaving any other distros untouched:

```shell
wsl -t AlmaLinux-10
```

To shut down all currently running WSL 2 distros at once:

```shell
wsl --shutdown
```

## Next step

Continue to [Virtualhosts](virtualhosts/overview.md) to start hosting your projects.

## FAQ

**Q: What if `AlmaLinux-10` doesn't appear in the tab selector dropdown?**

A: Confirm it's actually installed by running:

```shell
wsl -l -v
```

If it's missing from the list entirely, revisit [Install AlmaLinux 10](setup/installation.md).
Restarting Windows Terminal after installation also helps it pick up the new tab profile.

**Q: What if `wsl -d AlmaLinux-10` fails to start?**

A: Run the below command to confirm the distro is installed and check its current state:

```shell
wsl -l -v
```

If another distro is using the same resources, stop it first with the below command, then retry:

```shell
wsl -t <distro-name>
```

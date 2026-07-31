# Running AlmaLinux 9 on WSL 2

> If you are not using WSL 2, connect via SSH with [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/).

Open `Windows Terminal`.

Start **AlmaLinux 9** by executing:

```shell
wsl -d AlmaLinux-9
```

OR

Locate the app selector dropdown in `Windows Terminal`'s title bar and click `AlmaLinux-9`.
This will open a new tab connected to **AlmaLinux 9**.

> To run your applications using WSL 2, you always need to be connected to your **AlmaLinux 9** distribution.
> For this, all you need to do is to launch it from a terminal, file explorer or an IDE.
> Once launched, you can close the tool you launched it from.
> It will stay connected until you shut it down manually or reboot your computer.

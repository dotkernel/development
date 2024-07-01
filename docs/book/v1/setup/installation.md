# Installation

## Install AlmaLinux 9

Open Microsoft Store, in the search box type in: `AlmaLinux` and hit `Enter`.

From the results, select **AlmaLinux 9** - this will take you to **AlmaLinux 9**'s app page.

On this page, locate and click the `Install` button - this will download **AlmaLinux 9** WSL image on your system.

Once the download has finished, the `Install` button is replaced by an `Open` button - clicking it will open
`Windows Terminal`.

Here you will be asked to fill in your username (for example `dotkernel`):

```text
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

Next, you are prompted to enter a password to use with your username (you will not see what you are typing, that's a
security measure in Linux regarding passwords):

```text
Enter new UNIX username: dotkernel.
Changing password for user dotkernel.
New password:
```

Depending on the strength of your password, you might see one of the following messages (if you want to choose a
different password, hit `Enter` and you are taken back to previous step - else, continue with retyping your password).

```text
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary word
BAD PASSWORD: The password is a palindrome
```

Next, you are asked to retype your password:

```text
Retype new password:
```

Finally, you should see the following message:

```text
passwd: all authentication tokens updated successfully.
Installation successful!
[dotkernel@hostname:~]$
```

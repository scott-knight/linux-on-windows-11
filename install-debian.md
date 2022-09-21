# Installing Ubuntu

This document demonstrates how to install the current supported release of Ubuntu on Windows 10.

## Download and Install Ubuntu

You can download and install any version of Ubuntu, there is a way to do it via the console. However, the easiest way to install Ubuntu is to download it from the Microsoft Store. 

1. Open the Microsoft Store
2. In the search field, type `ubuntu`

![store-ubuntu](https://user-images.githubusercontent.com/516548/112919729-46799280-90cd-11eb-849e-0e7495bb1f3f.png)

<br/>

3. Select the first option, `Ubuntu app`. This will install the latest LTS of Ubuntu. 
4. Once installed, click the `Launch` button. This will open a new termial session. 
5. In the terminal session, it will ask for a user name. Input any username you want. I use the first inital of my first name in combination with my last name `sknight`.
6. After creating your user name, it will ask for a password. Choose a password and hit enter. It will ask you to re-enter the password again to verify.
7. Once done with setting up your user name and password, it will open a Bash console session, you will in the Ubuntu instance in your home directory. 

<br/>

## Opening an Ubuntu instance

Now that you have Ubuntu installed, you can right-click on the Windows Terminal icon in the `Start Menu`. You will see Ubuntu in the list.

![terminal-ubuntu](https://user-images.githubusercontent.com/516548/112920657-03b8ba00-90cf-11eb-928f-0bc4743efed5.png)

<br/>

## Connecting VSCode to your Ubuntu instance

1. In the Ubuntu console, type the following:

```sh
code .
```

This will attempt to open VSCode and connect VSCode to your WSL2, Ubuntu instance. I'm a little hazy on what happens after it connects, but you should be able to see the files of your home directory.

<br/>

2. Close VSCode and the terminal 

The next step, [update the settings of Windows Terminal](https://github.com/scott-knight/ubuntu-on-windows-setup/blob/main/configure-windows-terminal.md).

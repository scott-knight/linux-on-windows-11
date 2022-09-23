# Configure Windows Terminal

Once you have Ubuntu installed you will need to configure Windows Terminal to use the correct Powerline icons. Again, you don't have to do this step if you aren't using Windows Terminal powershell or a zsh theme that doesn't require them.

<br/>

## Terminal Settings

1. To open the `Windows Terminal` settings, click the `Start Menu` icon, and click the `Terminal` icon 

![Terminal Icon Start Menu](https://user-images.githubusercontent.com/516548/192063544-fc6a4fdf-ce4b-4eb4-a160-502a99e01b2d.png)

<br/>

2. Once `Windows Terminal` opens, click the down arrow (carret) to reveal the menu with the `settings` option. You can also see the shortcut next to each option. You can use those shortcut keys to open what you need in the future. However, for this setup, simply click `settings`. 

![terminal-settings](https://user-images.githubusercontent.com/516548/112922923-ef76bc00-90d2-11eb-9396-5bbdaee57106.png)

<br/>

4. If you are using VSCode, after you click `settings`, a VSCode will open showing you the settings. 

![all-settings](https://user-images.githubusercontent.com/516548/112923040-24830e80-90d3-11eb-83c9-5f46a4fdc3cc.png)

<br/>

5. Scroll down to the `profile > defaults > list` section. In each section where you would like to use the `powerline fonts`, append the following attribute to each item in the list:

```json
"fontFace":  "Cascadia Code PL"
```

<br/>

6. When you have appened each item in the list, it should look like this:

![font-face](https://user-images.githubusercontent.com/516548/112923270-99eedf00-90d3-11eb-9f97-9c6a19c63da9.png)

<br/> 

7. You can set the default `Windows Terminal` shell by copying a `guid` from the desired shell in the list an pasting it in the `defaultProfile` attribute of the settings file

![settings-default-profile](https://user-images.githubusercontent.com/516548/112922863-d66e0b00-90d2-11eb-9b24-99e4fb533c5c.png)

<br/>

In the example you can see that I pasted the `ubuntu` shell `guid` in the `defaultProfile` attribute. 

<br/>

After you save the settings, when you click on the `Windows Terminal` icon in the `Start Menu`, it will automatically open desired shell.

## Change the Starting Directory

In the ubuntu profile, add the following:

```json
 "startingDirectory": "\\\\wsl$\\Ubuntu\\home\\sknight"
 ```
 
Change the name `sknight` to your home directory name, then save the file

Now you are ready to [customize Ubuntu](https://github.com/scott-knight/ubuntu-on-windows-setup/blob/main/configure-ubuntu.md)
 

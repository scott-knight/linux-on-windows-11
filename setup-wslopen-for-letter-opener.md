In a rails project, it's possible to use the [letter_opener gem](https://github.com/ryanb/letter_opener) for posting email in the dev environment. The letter_opener gem uses the launchy gem to launch/post generated email to a browser tab. On a Mac (or possibly a native Linux setup) the browser is set by the system and launchy just works. However, with WSL we have to make the browser connection for launchy. In this example, we will point to location on the Windows drive for Chrome (assuming you have Chrome installed).

Chrome is installed on the `C` drive in the following location: `C:\Program Files\Google\Chrome\Application\chrome.exe`. This needs to be translated to a WSL2 path: `/mnt/c/Program\ Files/Google/Chrome/Application/chrome.exe`. In the code snippet below, you can replace `/mnt/c/Program\ Files/Google/Chrome/Application/chrome.exe` with the browser of your choice, but you'll need the path of the browser executable.

We need to create a couple of files to get everything to work. In an Linux (Ubuntu) shell, run the following:

```zsh
mkdir $HOME/.local && mkdir $HOME/.local/bin
```

<br/>

To add Chrome as the browser used by letter_opener (for a different browser, change the path in the snippet), run the following:

```zsh
echo '#!/usr/bin/zsh' >> $HOME/.local/bin/wslopen && echo '/mnt/c/Program\ Files/Google/Chrome/Application/chrome.exe "file://$(wslpath -m ${1/"file://"/})"' >> $HOME/.local/bin/wslopen && chmod +x $HOME/.local/bin/wslopen
```

<br/>

### Settings for `.zshrc`

Settings for letter_opener may already exist in your `.zshrc` file. If it's in your `.zshrc` file, it looks like this:

```zsh
# .local/bin -- used with letter_opener
export PATH="${HOME}/.local/bin:$PATH"
export BROWSER="wslopen"
```

<br>

If this code isn't already in your `.zshrc` file, you can either copy/paste the snippet above, or, you can run the following in the console:

```zsh
echo '# .local/bin -- used with letter_opener' >> .zshrc && echo 'export PATH="${HOME}/.local/bin:$PATH"' >> .zshrc && echo 'export BROWSER="wslopen"' >> .zshrc
```

Once added, reload the shell.

<br/>

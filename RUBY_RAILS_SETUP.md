[Home](https://github.com/scott-knight/linux-on-windows-11) | [Customize Debian](customize-debian.md#setup-for-ruby-and-rails) | [Customize Ubuntu](customize-ubuntu.md#setup-for-ruby-and-rails)

# Setting up Ruby and Rails

This doc explains how to setup Ruby via RBENV, and Yarn (assuming you are using Yarn in your project). It also contains notes for resolving issues found while setting up a Rails project.

<br/>

## Install Ruby via RBENV

Install the Ruby you want:

```sh
cd $HOME && rbenv install 3.1.3 --verbose
```
<br/>

### Issues When Installing Ruby on Ubuntu

If chose to install Ubuntu instead of Debian, you may run into issues installing Ruby because of SSL incompatibility, and you'll need to use Brew OpenSSL. Run the following:

```sh
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)" rbenv install 3.1.3 --verbose
```

<br/>

Once Ruby installs, you will want to update `ruby-gems` and bundler:

```sh
gem update --system && gem install bundler
```
<br/>

## Install Yarn

NVM no longer honors global installs of yarn (because of some Node issue). You'll need to install Yarn in the node version you are running. I also ran into an issue when using earlier versions of node while trying to run yarn. I ended up having use the current node to get yarn to work as expected.

To install yarn, run the following:

```sh
npm install --global yarn
```

<br/>

## Clone Your Repo - Install Yarn Libraries

Once you have your project downloaded, and yarn libraries are installed, you can see which libraries are outdated by running the following:

```zsh
yarn outdated
```

To upgrade outdated libraries, run the following:

```zsh
yarn upgrade --latest
```

<br/>

## Setting up Rails

This section covers issues I've run into while trying to setup Rails on WSL Linux installations. Make sure you have PostgreSQL running before you attempt to install/bundle gems for a Rails project.

### Installing Gems

Once you've navigated to your downloaded project code, you can install the project gems by running the following:

```zsh
bundle
```

If successful, the project should be ready to run. I ran into issues running `bundle`, for the `pg` and `ffi` gems. If you have the same issues, follow the instructions below to install those gems. Once installed, you can re-run `bundle` to complete the project setup.

<br/>

### Installing Nokogiri (ruby 3.2.x -- working on this one still)

If you run into an issue with [installing Nokogiri](https://nokogiri.org/tutorials/installing_nokogiri.html#macos-error-use-of-undeclared-identifier-lzma_ok), you will need to run the following:

```sh
brew install libxml2
gem install nokogiri -- --use-system-libraries \
    --with-xml2-include=$(brew --prefix libxml2)/include/libxml2
```

<br/>

### Installing PG

Brew will install Postgres, but it doesn't supply the system compiler that gem pg needs to install. To find the sytem `pg_config` run the following:

```zsh
sudo find / -name pg_config
```

<br/>

You will see something like this

> /home/linuxbrew/.linuxbrew/Cellar/postgresql@15/15.1/bin/pg_config

> /usr/bin/pg_config

<br/>

You want to use the system's `pg_config` and not the brew install

```zsh
gem install pg -- --with-pg-config=/usr/bin/pg_config
```

<br/>

### Installing libffi for Bootsnap gem (Debian)

Rails uses bootsnap which uses libffi. We installed `libffi-dev` in a previous step. However, Bootsnap may have an issue running when we spin up Rails. If that happens, you may see this error that looks something like this:

> LoadError: libffi.so.8: cannot open shared object file: No such file or
> directory - /home/sknight/dev.projects/jobsauce-app/.gems/gems/ffi-1.15.5/lib/ffi_c.so

This means that the system library is unable to use the system ffi installed by default, and we need to install a specific version for our Rails app (in our example, we need version 1.15.1).

To install the specific version needed (as outlined in the error), and to get the app to ignore the system version, you do the following:

```sh
gem install ffi --version '1.15.5' -- --disable-system-libffi
```

OR you can simply install `ffi`, disabling the system version

```
gem install ffi -- --disable-system-libffi
```

NOTE: With Ruby 3.2.0 -- after updating and installing, there is an issue that arises: `cannot find 'argon2_wrap' library`. This is an issue for which I could not find an answer (12/25/2022)

<br/>

### Setup for letter_opener

In my rails projects, I use the [letter_opener gem](https://github.com/ryanb/letter_opener) for posting email in the dev environment. The letter_opener gem uses the launchy gem to launch/post generated email to a browser tab. On a Mac (or possibly a native Linux setup) the browser is set by the system and launchy just works. However, with WSL we have to make the browser connection for launchy. In this example, we will point to location on the Windows drive for [Brave Browser](https://brave.com)

Brave is installed on the `C` drive in the following location: `C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe`. This needs to be translated to a WSL2/Debian path: `/mnt/c/Program\ Files/BraveSoftware/Brave-Browser/Application/brave.exe`.

We need to create a couple of files to get everything to work:

```zsh
mkdir $HOME/.local/bin && touch $HOME/.local/bin/wslopen
```

<br/>

In the code snippet below, you can replace `/mnt/c/Program\ Files/BraveSoftware/Brave-Browser/Application/brave.exe` with the browser of your choice. Once you have the path to your chosen browser, run the following:

```zsh
echo '#!/usr/bin/zsh' >> $HOME/.local/bin/wslopen
echo '/mnt/c/Program\ Files/BraveSoftware/Brave-Browser/Application/brave.exe "file://$(wslpath -m ${1/"file://"/})"' >> $HOME/.local/bin/wslopen
chmod +x $HOME/.local/bin/wslopen
```

<br/>

If you haven't already (it's already in [.zshrc](ZSHRC.md)), copy the following to `.zshrc`:

```zsh
# .local/bin
export PATH="${HOME}/.local/bin:$PATH"
export BROWSER="wslopen"
```

Then type `reload` to reload the shell.

<br/>

### Setup the DB locally

Once you have all the gems installed and you can execute `rails`, you will be ready to setup the DBs. To setup the DB's run the following:

```zsh
rails db:create db:schema:load db:migrate
```

<br/>

If you would like to completely reload the DBs, run the following:

```zsh
rails db:drop db:create db:schema:load db:migrate
```

<br/>

Once finished, you should be ready to run your local environment.

<br/><br/>

### Rails Issues

When you pull your repo code, you may run into an issue where rails is not executable:

> Permission denied - bin/rails (Errno::EACCES)

<br/>

To fix this, run the following:

```zsh
chmod u+x bin/rails
```

OR, if you have aliases set up

```
alias rfix="chmod u+x bin/rails"
```

<br/>

[Home](https://github.com/scott-knight/linux-on-windows-11) | [Customize Debian](customize-debian.md#setup-for-ruby-and-rails) | [Customize Ubuntu](customize-ubuntu.md#setup-for-ruby-and-rails)

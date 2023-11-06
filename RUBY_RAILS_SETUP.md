[Home](README.md) | [Customize Ubuntu](customize-ubuntu.md#setup-for-ruby-and-rails)

# Setting up Ruby and Rails

This doc explains how to setup Ruby via RBENV, and Yarn (assuming you are using Yarn in your project). It also contains notes for resolving issues found while setting up a Rails project.

You will need to make sure that RBENV is installed with plugins. At the console, run the following:

```zsh
reload_rbenv
```

<br/>

## Install Ruby via RBENV

Install the Ruby you want:

```sh
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl)" rbenv install 3.2.2 --verbose
```

<br/>

## Updating rubygems-update

Once Ruby installs, you will want to update `rubygems-update` and bundler:

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

<br/>

### Installing Gems

Once you've navigated to your downloaded project code, you can install the project gems by running the following:

```zsh
bundle
```

If successful, the project should be ready to run. I ran into issues running `bundle`, for the `pg` and `ffi` gems with Debian. If you have the same issues, follow the instructions below to install those gems. Once installed, you can re-run `bundle` to complete the project setup.

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

[Home](README.md) | [Customize Ubuntu](customize-ubuntu.md#setup-for-ruby-and-rails)

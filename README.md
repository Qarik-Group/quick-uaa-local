# Quaa - Quick UAA on local machine

This project is dedicated to making it easy to bring up a quick Cloud Foundry UAA on your local machine.

A pre-built Apache Tomcat and UAA war file are downloaded and installed. Your own requirements are Java 1.8+, and bash. You can also optionally use a local PostgreSQL (such as [Postgres.app](https://postgresapp.com/) for MacOS).

The name of the helper application is `quaa`.

## Run UAA locally

There are two paths to quickly bootstrap a dev/test UAA on your local machine:

1. `brew install` on MacOS
    ```plain
    brew install starkandwayne/cf/quaa
    ```
1. `git clone` of this repo:

    First, clone this repo.

    ```plain
    git clone https://github.com/starkandwayne/quick-uaa-local ~/workspace/quick-uaa-local
    cd ~/workspace/quick-uaa-local
    ```

    If you have `direnv` installed, you will be asked to `direnv allow` and then you will see the dependent CLIs (`uaa`, and `bosh`) downloaded into the `bin/` folder.

    If you do not see this, instead manually source the `quaa env` output to preinstall the dependent CLIs into your local path:

    ```plain
    eval "$(bin/quaa env)"
    ```

Using either method above you will now have `quaa` in your `$PATH`.

To run the UAA:

```plain
quaa up
```

This will automatically download Apache Tomcat, the `bosh` CLI used for generating secrets, encyption keys, and certificates for your UAA, and then run it locally on Java 1.8/Tomcat.

This is a long-running process in your terminal. Ctrl-C or similar to exit.

Once deployed, visit the UAA home page for users to login http://localhost:8080.

You can see the generated `uaa_admin` client secret, and complimentary `admin` user:

```plain
quaa info
```

To target and authorize the [`uaa` CLI](https://github.com/cloudfoundry-incubator/uaa-cli) (which was automatically downloaded by either of the installation steps above):

```plain
quaa auth-client

uaa clients
uaa users
uaa groups
```

If you used the `git clone` installation process, you can make the `quaa` and `uaa` CLIs available anywhere by eval-ing the `quaa env` output:

```plain
eval "$(~/workspace/quick-uaa-local/bin/quaa env)"
```

## PostgreSQL

The `quaa up` command will store data in memory, which will be lost upon termination.

You can use a local PostgreSQL to persist data over time.

```plain
quaa up --postgresql
```

To see various options try:

```plain
quaa up -h
```

## Upgrading

The `quaa up` command can be run in future to upgrade.

## Create Users

For each staff member you can easily create a new UAA user:

```plain
uaa create-user drnic \
  --email drnic@starkandwayne.com \
  --givenName "Dr Nic" \
  --familyName "Williams" \
  --password drnic_secret
```

You can assign them to groups (which gives them access to authorized scopes):

```plain
uaa add-member uaa.admin drnic
```

## Upgrade UAA

If you used `brew install` installation, then continue to use `brew` CLI to upgrade:

```plain
brew upgrade quaa
quaa up
```

If you used `git clone` installation, then pull the lastest commits from the `master` branch and run `quaa up` again.

```plain
cd ~/workspace/quick-uaa-local
git pull
quaa up
```

## Homebrew Recipe

The `brew install starkandwayne/cf/quaa` homebrew recipe is located at https://github.com/starkandwayne/homebrew-cf/blob/master/quaa.rb

## Run inside Docker

To test this project within different OS/different assumptions you can use Docker:

```plain
docker run -ti -v $PWD:/quick-uaa-local -p 8080:8080 ${image:-openjdk:8-jre} /quick-uaa-local/bin/quaa up --clean --memory
```
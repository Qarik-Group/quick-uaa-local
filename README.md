# Quaa - Quick UAA on local machine

This project is dedicated to making it easy to bring up a quick Cloud Foundry UAA on your local machine.

A pre-built Apache Tomcat and UAA war file are downloaded and installed. Your own requirements are Java 1.8+, bash, and a locally running PostgreSQL (such as [Postgres.app](https://postgresapp.com/) for MacOS).

The name of the helper application is `quaa`.

## Deploy UAA to Cloud Foundry

To quickly bootstrap a secure, production-grade UAA to any Cloud Foundry:

First, clone this repo.

```plain
git https://github.com/starkandwayne/quick-uaa-local ~/workspace/quick-uaa-local
cd ~/workspace/quick-uaa-local
```

If you have `direnv` installed, you will be asked to `direnv allow` and then you will see the dependent CLIs (`uaa`, and `bosh`) downloaded into the `bin/` folder.

If you do not see this, instead manually source the `quaa env` output to preinstall the dependent CLIs into your local path:

```plain
eval "$(bin/quaa env)"
```

To download Apache Tomcat, generate secrets, encyption keys, certificates for your UAA, and run it locally on Java 1.8/Tomcat:

```plain
quaa up
```

Note, the `quaa up` command can be run in future to upgrade. The flags provided above will be cached in `vars.yml` and not be required to be provided again.

Once deployed, visit the UAA home page for users to login http://localhost:8080.

You can see the generated `uaa_admin` client secret, and complimentary `admin` user:

```plain
$ quaa info
```

To target and authorize the [`uaa` CLI](https://github.com/cloudfoundry-incubator/uaa-cli):

```plain
quaa auth-client

uaa clients
uaa users
uaa groups
```

To use the `quaa` and `uaa` CLIs anywhere, eval the `quaa env` output:

```plain
eval "$(~/workspace/quick-uaa-local/bin/quaa env)"
quaa auth-client

uaa clients
uaa users
uaa groups
```


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

To upgrade, pull the lastest commits from the `master` branch and run `quaa up` again.

```plain
cd ~/workspace/quick-uaa-local
git pull
quaa up
```

## Destroy UAA

To tear down your Apache Tomcat/UAA:

```plain
quaa down
```
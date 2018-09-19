# Ops files for UAA configuration

You can customize your UAA by applying operator files. Create an `operators/uaa/` folder and any `.yml` files in it will be automatically applied to your UAA YAML configuration.

This folder contains production-ready operator files and examples.

For example, to enable multi-factor authentication/two-facto authentication, copy the `7-enable-mfa-google-auth.yml` file into `operators/` folder, and run `u up` to apply changes.

```plain
mkdir -p operators/uaa/
cp ops-files/uaa/7-enable-mfa-google-auth.yml operators/uaa/
u up
```

Some operator files will require additional `vars.yml` variables. Manually edit your `vars.yml` file and add your own values.

Test that your operator files will correctly apply to the base `src/uaa.yml` and other operators using the `u uaayml` command.

```plain
u uaayml
```

| Name | Purpose | Notes |
|:---  |:---     |:---   |
| **Branding** | | |
| [`4-branding-example.yml`](4-branding-example.yml) | Branding example | Copy and modify for your organization |
| [`5-google-analytics.yml`](5-google-analytics.yml) | Google Analytics | Configure your Google Analyics code |
| **Sign up / Login** | | |
| [`7-self-service.yml`](7-self-service.yml) | Self-service signup and password reset | Users can create their own account and reset their password. Requires SMTP server. |
| [`7-enable-mfa-google-auth.yml`](7-enable-mfa-google-auth.yml) | Require MFA | Users will be required to setup Google Authenticator/Authy when they nexted login. |
| [`7-google-oidc-provider.yml`](7-google-oidc-provider.yml) | Login with Google | Users can sign up and login with their Google account. Requires Google Compute credentials. |
| **Operations** | | |
| [`2-db-connections.yml`](2-db-connections.yml) | Scale up DB connections | Increase from 4 max active connections to database |
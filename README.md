# Test SAML 2.0 IdP

[Docker](https://www.docker.com/) container with a plug 'n play [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) 2.0 [Identity Provider](https://en.wikipedia.org/wiki/Identity_provider_(SAML)) (IdP) for development and testing.

[![Build status](https://github.com/qtsolv/docker-test-saml-idp/workflows/cd/badge.svg)](https://github.com/qtsolv/docker-test-saml-idp/)
[![Image size](https://ghcr-badge.egpl.dev/qtsolv/docker-test-saml-idp/size)](https://github.com/qtsolv/docker-test-saml-idp/)
[![Image size](https://ghcr-badge.egpl.dev/qtsolv/docker-test-saml-idp/tags?trim=major)](https://github.com/qtsolv/docker-test-saml-idp/)

This project was forked from [kristophjunge/docker-test-saml-idp](https://github.com/kristophjunge/docker-test-saml-idp) to bring support for native `arm64` container.

## Usage

Run the [Docker](https://www.docker.com/) image as below:

```
docker run --name=local-test-idp \
  -p 8080:8080 \
  -p 8443:8443 \
  -e SIMPLESAMLPHP_SP_ENTITY_ID=http://app.example.com \
  -e SIMPLESAMLPHP_SP_ASSERTION_CONSUMER_SERVICE=http://app.example.com/acs/callback \
  -e SIMPLESAMLPHP_SP_SINGLE_LOGOUT_SERVICE=http://app.example.com/sls/callback \
  -d ghcr.io/qtsolv/docker-test-saml-idp:master
```

You can access the [SimpleSAMLphp](https://simplesamlphp.org) web interface of the IdP under `http://localhost:8080/simplesaml`. The admin password is `secret`.

There are two static users configured in the IdP with the following data:

| UID | Username | Password | Group | Email |
|:---:|:--------:|:--------:|:-----:|:-----:|
| 1 | user1 | user1pass | group1 | user1@example.com |
| 2 | user2 | user2pass | group2 | user2@example.com |

However, you can define your own users by mounting a configuration file:

```
-v /users.php:/var/www/simplesamlphp/config/authsources.php
```

To configure the IdP on your SP implementation, you may use below settings:

```php
$metadata['http://localhost:8080/simplesaml/saml2/idp/metadata.php'] = array(
    'name' => array(
        'en' => 'Test IdP',
    ),
    'description' => 'Test IdP',
    'SingleSignOnService' => 'http://localhost:8080/simplesaml/saml2/idp/SSOService.php',
    'SingleLogoutService' => 'http://localhost:8080/simplesaml/saml2/idp/SingleLogoutService.php',
    'certFingerprint' => '119b9e027959cdb7c662cfd075d9e2ef384e445f',
);
```

## License

Please see the [LICENSE](LICENSE) file.

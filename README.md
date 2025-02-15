# homeassistant-auth-authelia

Authenticate Home Assistant users against an Authelia instance.

This script uses the command line auth_providers by [Home Assistant](https://home-assistant.io) to authenticate users against an [Authelia](https://www.authelia.com/) instance. It is also possible to to restrict access to certain groups.

## Setup

1. Two factor authentication for your home assistant URL has to be disabled in Authelia.

    ```yaml
    access_control:
      rules:
        - domain: 'https://homeassistant.example.com'
          policy: one_factor
    ```

2. Enable the _Legacy endpoint_ in Authelia:

    ```yaml
    server:
      endpoints:
        authz:
          legacy:  # used by homeassistant
            implementation: 'Legacy'
            authn_strategies: []
    ```

3. Put _auth_authelia.sh_ to any path that is accessible by home assistant (e.g. /config/bin/auth_authelia.sh).

4. Configure Home Assistant to use a command line authentication provider by adding the following to your configuration.yaml:

    ```yaml
    homeassistant:
      auth_providers:
        - type: command_line
          command: /config/bin/auth_authelia.sh
          args:
            ["https://auth.example.com", "https://homeassistant.example.com", "homeassistant_users", "homeassistant_admins"]
          meta: true
        - type: homeassistant
    ```

That's it.

On the login screen Home Assistant offers you the command line authentication provider by default. You will still be offered to login with "Home Assistant Local".

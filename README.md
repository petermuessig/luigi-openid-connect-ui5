# WORK IN PROGRESS !
# luigi-openid-connect-ui5 
A simple example setting up a Luigi based microfrontend application with ui5 with openid connect as authentication mechanism


## OpenUI5 External Library Requirements

The following external libraries are included in the ui5.yaml file as resources
 - @luigi-project/plugin-auth-oidc
 - oidc-client
 - @luigi-project/core
 - @luigi-project/client

## Setting the Luigi Auth properties for OIDC configuration
You need to use the luigi auth plugins in your Luigi Config file.
Change the javascript config import type to a module to be able to import external libraries there:

Modify this line accordingly in index.html to treat it as a module so imports are allowed:

`<script type="module" src="/luigi-config.js"></script>`


Then using the oidc-mockserver provided the config should look like this:
```
import * as oidcProvider from './assets/auth-oidc/plugin.js';

Luigi.setConfig({
  navigation: {
    nodes: () => []
  },
  routing: {...},
  settings: {...},
  auth: {
    use: 'openIdConnect',
    openIdConnect: {
      idpProvider: oidcProvider,
      authority: 'http://localhost:4011/',
      client_id: 'authorisation-code-pkce-mock-client', // example oidc-mockserver client id
      response_type: "code", // for PKCE
      response_mode: "fragment", // change between `query` and `fragment`
      scope: 'openid profile email',
      post_logout_redirect_uri: '/logout.html',
      automaticSilentRenew: true,
      accessTokenExpiringNotificationTime: 60,
    },
    disableAutoLogin: false
  }
});

```


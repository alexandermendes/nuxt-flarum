# nuxt-flarum

A Nuxt module for Flarum SSO

## Setup

The setup process includes stages for both Nuxt.js and Flarum.

### Configure Nuxt.js

Install with npm:

```
npm install @nuxtjs/flarum
```

Update *nuxt.config.js*:

```
module.exports = {
  modules: [
    '@nuxtjs/flarum',
  ],

  flarum: {
    url: 'https://discuss.example.com',
    sessionCookieDomain: '.example.com',
    apiKey: 'master-api-key',
    salt: 'random-secret-string'
  }
}
```

## Configure Flarum

Create a random token and put it in the `api_keys` table of your Flarum
database.

Install [flarum-ext-sso](https://github.com/fabwu/flarum-ext-sso):

```
composer require wuethrich44/flarum-ext-sso
```

Activate the extension and fill in the redirect URLs for login, signup and
logout.

Configure the server (options for Apache shown):

```
# Set headers
Header always set Access-Control-Allow-Origin "https://www.example.com"
Header always set Access-Control-Allow-Credentials true
Header always set Access-Control-Allow-Methods "GET, POST, PUT, PATCH, DELETE, OPTIONS"
Header always set Access-Control-Allow-Headers "Origin, Content-Type, Authorization, Set-Cookie"

# Return a 200 response for OPTIONS requests
RewriteEngine On
RewriteCond %{REQUEST_METHOD} OPTIONS
RewriteRule ^(.*)$ $1 [R=200,L]
```

Once the server is restarted, SSO should be enabled.

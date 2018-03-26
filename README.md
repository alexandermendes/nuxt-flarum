# nuxt-flarum

A Nuxt module for Flarum SSO

## Setup

Install with npm:

```
npm install @nuxtjs/flarum
```

nuxt.config.js

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

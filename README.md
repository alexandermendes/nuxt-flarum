# nuxt-flarum

[![npm version](https://badge.fury.io/js/nuxt-flarum.svg)](https://badge.fury.io/js/nuxt-flarum)

A Nuxt module for Flarum SSO.

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

### Configure Flarum

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

## Usage

```
<template>
  <main>
    <input v-model="username">
    <input v-model="email">
    <input v-model="password">
    <button @click="signin">Signin</button>
    <button @click="signout">Signout</button>
  </main>
</template>

<script>
  export default {
    data: {
      username: null,
      email: null,
      password: null
    },

    methods: {
      signin () {
        this.$flarum.signin(this.username, this.email)
      },

      signout () {
        this.$flarum.signout()
      }
    }
  }
</script>
```

Users are resolved by email address. If no user can be found with an email
address that matches one passed in `flarum.signin()` then a new user will be
created.

This means that if the user updates their email address in your main
application this update should also be passed to Flarum. Otherwise, when the
user next visits the Flarum site a new account created for them (using their
new email address). Here's an example of a function that can be used to
update a current user's email address:

``` js
updateEmail(oldEmail, newEmail) {
  return this.$flarum.getUserByEmail(oldEmail).then(flarumUser => {
    if (flarumUser) {
      this.$flarum.updateUser(flarumUser.id, {
        email: newEmail
      })
    }
  }).catch(err => {
    this.$nuxt.error(err)
  })
}
```
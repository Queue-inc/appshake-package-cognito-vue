# appshake package cloudinary vue

## Installation

```
appshake package:add Queue-Inc/appshake-package-cognito-vue
```

## Setup environment variables

### Cloudinary variables

```
appshake set VUE_APP_COGNITO_REGION your_cognito_region
```

```
appshake set VUE_APP_COGNITO_USERPOOL_ID your_cognito_userpool_id
```

```
appshake set VUE_APP_COGNITO_IDPOOL_ID your_cognito_idpool_id
```

```
appshake set VUE_APP_COGNITO_CLIENT_ID your_cognito_web_client_id
```

## Import to your view file

### Web
```App.vue
<template lang="pug">
  #app
    cognito-auth(
      @signIn="signInDone"
      @signUp="signUpDone"
      @sendLink="sendLinkDone"
    )
</template>
<script>
import CognitoAuth from '@P/components/organisms/CognitoAuth.vue'
export default {
  components: {
    CognitoAuth
  },
  methods: {
    signInDone (payload) {
      console.log(payload)
    },
    signUpDone (payload) {
      console.log(payload)
    },
    sendLinkDone (payload) {
      console.log(payload)
    }
  }
}
</script>
```

## Classes

### web/src/packages/organisms/CognitoAuth.vue

| Class name | target |
| :--- | :--- |
| .cognito-auth-wrapper | Wrapper of auth component |
| .cognito-auth-tab | Top tab to switch sign in and sign up |
| .cognito-auth-tab-item | Top tab item |
| .cognito-auth-content | Auth component content wrapper |
| .cognito-auth-input | Auth component input |
| .cognito-auth-button | Auth component button |
| .cognito-auth-text-button | Auth component text button |


## Events

### web/src/packages/organisms/CognitoAuth.vue

| Event | target |
| :--- | :--- |
| signIn | Fire when sign in successed |
| signUp | Fire when sign up successed |
| sendLink | Fire when send email for resetting password successed |
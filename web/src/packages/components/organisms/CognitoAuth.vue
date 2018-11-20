<template lang="pug">
.cognito-auth-wrapper
  //- tab - sign in ⇄ sign up
  .cognito-auth-tab
    .cognito-auth-tab-item(
      :class="{ 'active': selected === 'SignIn' }"
      @click="selectTab('SignIn')"
    ) Sign In
    .cognito-auth-tab-item(
      :class="{ 'active': selected === 'SignUp' }"
      @click="selectTab('SignUp')"
    ) Sign Up
  .cognito-auth-content
    //- sign in tab
    template(v-if="selected === 'SignIn'")
      input.cognito-auth-input(
        type="email"
        placeholder="Email"
        v-model="signInEmail"
      )
      input.cognito-auth-input(
        type="password"
        placeholder="Password"
        v-model="signInPassword"
      )
      button.cognito-auth-button(
        @click="signIn"
        :disabled="connecting"
      ) Sign In
      p.cognito-auth-text-button(
        @click="showForgetPassword = true"
      ) Forget Password
      //- forget password section
      template(v-if="showForgetPassword")
        input.cognito-auth-input(
          placeholder="Email"
          v-model="passwordEmail"
        )
        button.cognito-auth-button(
          @click="sendLink"
          :disabled="connecting"
        ) Send Link
    //- sign in tab
    template(v-if="selected === 'SignUp'")
      input.cognito-auth-input(
        placeholder="Email"
        v-model="signUpEmail"
      )
      input.cognito-auth-input(
        type="password"
        placeholder="Password"
        v-model="signUpPassword"
      )
      input.cognito-auth-input(
        type="password"
        placeholder="Password confirm"
        v-model="signUpPasswordConfirm"
      )
      button.cognito-auth-button(
        @click="signUp"
        :disabled="connecting"
      ) Sign Up
</template>
<script>
import Amplify from 'aws-amplify'
export default {
  data () {
    return {
      connecting: false,
      selected: 'SignIn',
      showForgetPassword: false,
      signInEmail: '',
      signInPassword: '',
      signUpEmail: '',
      signUpPassword: '',
      signUpPasswordConfirm: '',
      passwordEmail: ''
    }
  },
  created () {
    Amplify.configure({
      Auth: {
        identityPoolId: process.env.VUE_APP_COGNITO_IDPOOL_ID,
        region: process.env.VUE_APP_COGNITO_REGION,
        userPoolId: process.env.VUE_APP_COGNITO_USERPOOL_ID,
        userPoolWebClientId: process.env.VUE_APP_COGNITO_CLIENT_ID
      }
    })
  },
  methods: {
    selectTab (key) {
      this.selected = key
    },
    signIn () {
      // validation
      if (!this.signInEmail) return alert('Input your email address')
      if (!this.signInPassword) return alert('Input your password')
      // connection start
      this.connecting = true

      Amplify.Auth.signIn(this.signInEmail, this.signInPassword)
        .then(data => {
          this.connecting = false
        })
        .catch(err => {
          this.connecting = false
          alert(err.message)
        })
    },
    signUp () {
      // validation
      if (!this.signUpEmail) return alert('Input your email address')
      if (!this.signUpPassword) return alert('Input your password')
      if (this.signUpPassword !== this.signUpPasswordConfirm) return alert('Passwords does not match')
      // connection start
      this.connecting = true

      Amplify.Auth.signUp({
        username: this.signUpEmail,
        password: this.signUpPassword,
        attributes: {
          email: this.signUpEmail
        }
      })
        .then(data => {
          this.connecting = false
          alert('Your account successfully registered. Please check you inbox.')
          this.signUpEmail = this.signUpPassword = this.signUpPasswordConfirm = ''
        })
        .catch(err => {
          this.connecting = false
          alert(err.message)
        })
    },
    sendLink () {
      // validation
      if (!this.passwordEmail) return alert('Input your email address')
      // connection start
      this.connecting = true

      Amplify.Auth.forgotPassword(this.passwordEmail)
        .then(data => {
          this.connecting = false
          alert('Password reset link sent. Please check you inbox.')
          this.passwordEmail = ''
        })
        .catch(err => {
          this.connecting = false
          alert(err.message)
        })
    }
  }
}
</script>
<style lang="stylus">
.cognito-auth-wrapper
  display flex
  flex-direction column
  align-items stretch

.cognito-auth-tab
  height 40px
  display flex
  flex-direction row

.cognito-auth-tab-item
  flex 1
  font-size 24px
  box-sizing border-box
  display flex
  align-items center
  justify-content center
  &.active
    border-bottom solid 2.5px #000

.cognito-auth-content
  padding 24px
  display flex
  flex-direction column
  align-items stretch

.cognito-auth-input
  margin 40px 0
  outline none
  font-size 16px
  padding 4px 8px

.cognito-auth-button
  outline none
  border solid 1px #d0d0d0
  background-color #eeeeee
  font-size 24px
  margin 40px 0
  padding 4px 8px

.cognito-auth-text-button
  font-size 24px
  padding 4px 8px
  text-align center
</style>

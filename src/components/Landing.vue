<template>
  <div class="hello">
    <div>
      <img src="logo.png" alt="Thunderstack logo" width="300" height="300">
      <h1>Thunderstack</h1>
      <h3>Your Epic Weight Loss Dashboard</h3>
      <button class="btn btn-default" @click.prevent="signIn">Sign In With Blockstack</button>
      <p><em>Currently in development and not available on mobile devices</em></p>
    </div>
     <small class="creds">
      by <a href="https://www.twitter.com/thunderpounds">Thunderpounds</a>
    </small>
  </div>
</template>

<script>
export default {
  name: 'landing',
  data () {
    return {
      blockstack: window.blockstack
    }
  },

  methods: {
    signIn () {
      const blockstack = this.blockstack
      blockstack.redirectToSignIn()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  height: 75%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>

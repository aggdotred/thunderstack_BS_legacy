<template>
  <div class="hello">
    <div>
      <h1>Thunderstack</h1>
      <p>Epic Weight Loss Dashboard</p>
      <button class="btn btn-default" @click.prevent="signIn">Sign In With Blockstack</button>
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
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>

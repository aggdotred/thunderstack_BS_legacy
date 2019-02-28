<template>
  <div class="Dashboard">
    <div class="container">
      <div class="row">
        <Navbar :user="user"></Navbar>
        <div class="col-md-8 col-md-offset-2">
          <h2>
            <small>
              Your Weight Logs
            </small>
          </h2>
          <WeightLogForm :user="user"></WeightLogForm>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from './Navbar'
import WeightLogForm from './WeightLogForm'

export default {
  name: 'dashboard',
  components: { Navbar, WeightLogForm },
  props: ['user'],
  data () {
    return {
      blockstack: window.blockstack
    }
  },
  methods: {
    signOut () {
      this.blockstack.signUserOut(window.location.href)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
</style>

<template>
  <div class="Navbar">
    
    <h1 class="page-header">Thunderstack
      <img :src="user.avatarUrl() ? user.avatarUrl() : '/avatar-placeholder.png'" class="avatar">
      
      <small>
        <span class="sign-out">(<a href="#" @click.prevent="signOut">Sign Out</a>)</span>
      </small>
      
    </h1>
    
  </div>
</template>
<script>
  export default {
    name: 'Navbar',
    props: ['user'],
    data: () => {
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

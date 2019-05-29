<template>
  <div id="app">
    <base-spinner/>
    <router-view/>
  </div>
</template>

<script>
import BaseSpinner from './components/global/BaseSpinner'

export default {
  name: 'App',
  components: {
    BaseSpinner
  },
  mounted () {
    this.$firebase.auth().onAuthStateChanged(user => {
      window.uid = user ? user.uid : null
      console.log(window.uid)
      this.$router.push({ name: window.uid ? 'home' : 'login' })
      this.$root.$emit('Spinner::hide')
    })
  }
}
</script>

<style lang="scss">
  #app {
    min-height: 100vh;
    color: var(--light);
    background-color: var(--dark);
  }
</style>

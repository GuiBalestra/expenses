<template>
  <div id="app">
    <base-spinner></base-spinner>
    <button @click="mostrarSpinner">Mostrar Spinner</button>
    <!-- <router-view/> -->
  </div>
</template>

<script>
import BaseSpinner from './components/global/BaseSpinner';

export default {
  name: 'App',
  components: {
    BaseSpinner
  },
  methods: {
    mostrarSpinner() {
      this.$root.$emit('Spinner::show');
    }
  }
}
</script>


<style lang="scss">

</style>

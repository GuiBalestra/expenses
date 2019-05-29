<template>
  <form class="form-login" @submit.prevent="doLogin()">
    <div class="card">
      <div class="card-header text-center">
        <h1 class="mb-0">Expenses</h1>
      </div>
      <div class="card-body">
        <div class="form-group">
          <input
            type="email"
            class="form-control"
            placeholder="E-mail"
            required
            v-model="email">
        </div>
        <div class="form-group">
          <input
            type="password"
            class="form-control"
            placeholder="Senha"
            required
            v-model="password">
        </div>
        <button class="btn btn-primary w-100" :disabled="loading">
          <template v-if="loading">
            Entrando...
            <i class="fa fa-spinner fa-spin"></i>
          </template>
          <template v-else>
            Entrar
            <i class="fa fa-sign-in-alt"></i>
          </template>
        </button>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: 'Login',
  data: () => {
    return {
      email: 'gui@expenses.com',
      password: '123456',
      loading: false
    }
  },
  methods: {
    async doLogin () {
      const { email, password } = this
      this.loading = true
      try {
        const res = await this.$firebase.auth().signInWithEmailAndPassword(email, password)
        window.uid = res.user.uid
        // this.$router.push({name: 'home'})
      } catch (err) {
        console.log(err)
      }
      this.loading = false
    }
  },
  beforeRouteEnter (to, from, next) {
    next(vm => {
      if (window.uid) {
        vm.$router.push({ name: 'home' })
      }
    })
  }
}
</script>

<style lang="scss" scoped>
.form-login {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  h1 {
    font-size: 18pt;
  }
  .card {
    width: 30%;
    color: var(--darker);
  }
}
</style>

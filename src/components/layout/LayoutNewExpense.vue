<template>
  <div class="w-100 d-flex justify-content-center">
    <button class="btn btn-lg btn-outline-primary w-75" @click="showModal=true">
      <i class="fa fa-plus"></i>
      Novo Gasto
    </button>

    <form @submit.prevent="submit()">
      <div
        class="modal fade"
        :class="{show: showModal}"
        :style="{display: showModal ? 'block' : 'none'}"
      >
        <div class="modal-dialog modal-lg">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLive">Adicionar um novo gasto</h5>
              <button type="button" class="close" aria-label="Close" @click="closeModal()">
                <span aria-hidden="true">x</span>
              </button>
            </div>
            <div class="modal-body">
              <div class="row">
                <div class="form-group col-8">
                  <label for>Descrição</label>
                  <input type="text" class="form-control" required v-model="form.description">
                </div>
                <div class="form-group col-4">
                  <label for>Valor (R$)</label>
                  <input type="text" class="form-control" required v-model="form.value">
                </div>
                <div class="form-group flex-column col-12 d-flex align-items-center">
                  <input
                    type="file"
                    class="d-none"
                    accept="image/*"
                    ref="input"
                    @change="handleFile($event)"
                  >
                  <button
                    type="button"
                    class="btn w-50 btn-outline-secondary"
                    @click="openFileDialog()"
                  >Adicionar Comprovante</button>
                  <div class="mt-2" v-if="form.receipt">
                    {{ form.receipt.name }}
                    <button type="button" class="btn badge badge-light" @click="form.receipt = ''">
                      <i class="fa fa-trash text-danger"></i>
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" @click="closeModal()">Fechar</button>
              <button class="btn btn-primary">Incluir novo gasto</button>
            </div>
          </div>
        </div>
      </div>
      <div
        class="modal-backdrop fade"
        :class="{show: showModal}"
        :style="{display: showModal ? 'block' : 'none'}"
      ></div>
    </form>
  </div>
</template>

<script>
export default {
  data: () => ({
    showModal: false,
    form: {
      receipt: '',
      description: '',
      value: ''
    }
  }),
  computed: {
    fileName () {
      const { receipt } = this.form

      if (receipt) {
        const split = receipt.name.split('.')
        return `${split[0]}-${new Date().getTime()}.${split[1]}`
      } else {
        return ''
      }
    }
  },
  methods: {
    handleFile ({ target }) {
      this.form.receipt = target.files[0]
    },
    openFileDialog () {
      this.$refs.input.value = null
      this.$refs.input.click()
    },
    closeModal () {
      this.showModal = false
    },
    async submit () {
      let url = ''
      try {
        this.$root.$emit('Spinner::show')
        const ref = this.$firebase.database().ref(window.uid)
        const id = ref.push().key

        if (this.form.receipt) {
          const snapshot = await this.$firebase.storage().ref(window.uid).child(this.fileName).put(this.form.receipt)
          url = await snapshot.ref.getDownloadURL()
        }

        const payload = {
          id,
          ...this.form,
          receipt: url,
          createdAt: new Date().getTime()
        }

        ref.child(id).set(payload, err => {
          if (err) {
            console.error(err)
          } else {
            this.closeModal()
          }
        })
      } catch (err) {
        console.error(err);
      } finally {
        this.$root.$emit('Spinner::hide')
      }
    }
  }
}
</script>

<style scoped lang="scss">
.modal {
  color: var(--darker);
}
</style>

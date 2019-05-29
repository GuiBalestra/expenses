<template>
  <div class="w-100 d-flex justify-content-center">
    <button class="btn btn-lg btn-outline-primary w-75" @click="showModal=true">
      <i class="fa fa-plus"></i>
      Novo Gasto
    </button>

    <div class="modal fade"
    :class="{show: showModal}"
    :style="{display: showModal ? 'block' : 'none'}">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLive"></h5>
            <button type="button" class="close" aria-label="Close" @click="closeModal()">
              <span aria-hidden="true">x</span>
            </button>
          </div>
          <div class="modal-body">

          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="closeModal()">Close</button>
            <button type="button" class="btn btn-primary">Save Changes</button>
          </div>
        </div>
      </div>
    </div>
    <div class="modal-backdrop fade"
    :class="{show: showModal}"
    :style="{display: showModal ? 'block' : 'none'}">
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    showModal: false
  }),
  methods: {
    closeModal () {
      this.showModal = false
    }
  }
}
</script>

<style>
</style>

<template>
  <div class="modal" :class="{'is-active': open}">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">Credo notification</p>
        <button class="delete" @click="close"></button>
      </header>
      <section class="modal-card-body">
        <div class="content">
          <h1>About this report</h1>
          <h2>Payment</h2>
          <p>A payment of <strong>$20</strong> (0.00586BTC) will be sent to the address of the data providers upon access.</p>
          <h2>Verification</h2>
          <p>The Credo browser only shows data with valid digital signatures from the data providers. Visit our <a href="#">website</a> to learn more about our cryptographic proof.</p>
          <h2>Content</h2>
          <p>The following data has been verified:</p>
          <ul>
            <li v-for="point in data">{{point.name}}</li>
          </ul>
        </div>
      </section>
      <footer class="modal-card-foot">
        <a class="button is-info" @click="openReport">Continue</a>
        <a class="button" @click="close">Cancel</a>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: 'modal',
  props: {
    open: Boolean,
    data: Array
  },
  methods: {
    close: function () {
      this.$emit('close')
    },
    openReport: function () {
      this.$emit('open')
    }
  }
}
</script>

<style lang="scss" scoped>
  .content {
    max-width: 500px;
    margin: auto;
    p {
      color: black;
      text-align: left;
    }
    h2 {
      text-align: left;
    }
    li {
      color: black;
      text-align: left;
    }
  }
  .modal-card-foot {
    justify-content: center;
  }
</style>

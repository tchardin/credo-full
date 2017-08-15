<template>
  <div class="hero-body">
    <div class="container has-text-centered">
      <figure class="image is-128x128">
        <img
          :src="user.profile.image[0].contentUrl"
          class="profile">
      </figure>
      <h2 class="title is-5">{{user.profile.givenName}} {{user.profile.familyName}}</h2>
      <h3 class="subtitle is-7">{{user.zone_file.$origin}}</h3>
      <div class="box">
        <p class="menu-label">
          General
        </p>
        <table class="table is-fullwidth">
          <thead>
            <tr>
              <th>Type</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            <item
              v-for="data in userData.general"
              :model="data"
              :key="data.name">
            </item>
          </tbody>
        </table>
        <p class="menu-label">
          Payments
        </p>
        <table class="table is-fullwidth">
          <thead>
            <tr>
              <th>Type</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            <item
              v-for="data in userData.payments"
              :model="data"
              :key="data.name">
            </item>
          </tbody>
        </table>
        <a
          class="button is-info"
          :class="{'is-loading': loading}"
          @click="openModal">
          Open
        </a>
      </div>
      <modal
        :open="open"
        :data="userData.payments"
        v-on:close="closeModal"
        v-on:openReport="openData">
      </modal>
    </div>
  </div>
</template>

<script>
// todo:
// Pass a signed contract object with terms that could be stored on the user storage
var data = {
  general: [
    {
      name: 'Address',
      verified: false
    },
    {
      name: 'Phone #',
      verified: false
    },
    {
      name: 'Date of birth',
      verified: false
    }
  ],
  payments: [
    {
      name: 'Exelon',
      verified: true
    },
    {
      name: 'Verizon',
      verified: false
    },
    {
      name: 'Blue Cross Blue S.',
      verified: false
    }
  ]
}

import Item from './Item'
import Modal from './Modal'

export default {
  name: 'profile',
  props: {
    user: Object
  },
  components: {
    Item,
    Modal
  },
  data: function () {
    return {
      userData: data,
      open: false,
      loading: false
    }
  },
  methods: {
    openData: function () {
      var vm = this
      vm.loading = true
      // todo:
      // Send a request to the user endpoint with a key in the header
      // Read the data on the user drive
      // Query data provider and find public owner Address
      // Verify HMAC...
      setTimeout(function () {
        vm.loading = false
      }, 1000)
    },
    closeModal: function () {
      this.open = false
    },
    openModal: function () {
      this.open = true
    }
  }
}
</script>

<style lang="scss" scoped>

.image {
  margin: auto;
  padding: 1em;
}
.profile {
  border-radius: 50%;
}
.box {
  max-width: 600px;
  margin: auto;
}

</style>

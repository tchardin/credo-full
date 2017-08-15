<template>
  <div>
    <section class="hero is-info is-fullheight" v-if="owner">
      <!-- Hero header: will stick at the top -->
      <div class="hero-head">
        <header class="nav">
          <div class="container">
            <div class="nav-left">
              <a class="nav-item">
                <img src="./assets/credo-text.png" alt="Logo">
              </a>
            </div>
            <span class="nav-toggle">
              <span></span>
              <span></span>
              <span></span>
            </span>
            <div class="nav-right nav-menu">
              <a class="nav-item is-active" @click="newSearch">
                New Search
              </a>
              <a class="nav-item">
                Profile
              </a>
              <!-- Todo: add a Bitcoin wallet address and balance -->
              <a class="nav-item">
                Wallet
              </a>
              <span class="nav-item">
                <a class="button is-inverted" @click="signOut">
                  Sign out
                </a>
              </span>
            </div>
          </div>
        </header>
      </div>
      <!-- Profile content -->
      <profile
        v-if="customer"
        :user="customer">
      </profile>


      <!-- Search content: will be in the middle -->
      <div class="hero-body" v-else>
        <div class="container has-text-centered">
          <h1 class="title">
            Find a user to access their data
          </h1>
          <div class="field has-addons">
            <p class="control has-icons-left is-expanded">
              <input
                class="input"
                type="search"
                v-model="searchQuery"
                placeholder="Enter user id"
                @keyup.enter="searchCustomer">
              <span class="icon is-small is-left">
                <i class="fa fa-search"></i>
              </span>
            </p>
            <div class="control">
              <a class="button is-info is-static">
                .id
              </a>
            </div>
          </div>
        </div>
      </div>

      <!-- Hero footer: will stick at the bottom -->
      <div class="hero-foot">

      </div>
    </section>
    <section class="hero is-info is-fullheight" v-else>
      <div class="hero-head">
        <header class="nav">
          <div class="nav-left">
            <span class="nav-item">
              <img src="./assets/credo-text.png" alt="Logo">
            </span>
          </div>
        </header>
      </div>
      <div class="hero-body">
        <div class="container has-text-centered">
          <a class="button is-inverted" @click="signIn">Sign in</a>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
const blockstack = require('blockstack')
import Profile from './components/Profile'

export default {
  name: 'app',
  data () {
    return {
      owner: null,
      searchQuery: '',
      noUser: false,
      customer: null
    }
  },
  components: {
    Profile
  },
  methods: {
    searchCustomer: function () {
      var username = this.searchQuery
      return fetch(`https://core.blockstack.org/v2/users/${username}`, {
        method: 'GET'
      })
      .then((response) => response.json())
      .then((responseJson) => {
        console.log(responseJson)
        this.customer = responseJson[Object.keys(responseJson)[0]]
      })
      .catch((err) => console.log(err))
    },
    newSearch: function () {
      this.customer = null
    },
    toggleLogo: function () {
      if (this.owner === '/static/img/ford-logo.ca7fda1.png') {
        this.owner = '/static/img/citi-logo.8a70abb.png'
      } else {
        this.owner = '/static/img/ford-logo.ca7fda1.png'
      }
    },
    // Redirects to the Blockstack browser
    signIn: function () {
      blockstack.redirectToSignIn()
    },
    // Removes the user auth from local storage
    signOut: function () {
      blockstack.signUserOut(window.location.href)
      this.owner = null
    }
  },
  mounted () {
    if (blockstack.isUserSignedIn()) {
      this.owner = blockstack.loadUserData()
    } else if (blockstack.isSignInPending()) {
      blockstack.handlePendingSignIn()
      .then((userData) => {
        window.location = window.location.origin
      })
    }
  }
}
</script>

<style lang="scss">
  .field.has-addons {
    max-width: 600px;
    margin: auto;
  }

  @import '../node_modules/bulma/sass/utilities/initial-variables';

  $blue: #083D77;

  @import '../node_modules/bulma/bulma.sass';
</style>

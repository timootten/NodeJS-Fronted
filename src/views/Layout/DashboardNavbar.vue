<template>
  <base-nav
    container-classes="container-fluid"
    class="navbar-top navbar-expand p-4"
    :class="{'navbar-dark': type === 'default'}"
  >
  
    <a href="#" aria-current="page" class="h4 mb-0 text-white text-uppercase d-none d-lg-inline-block active router-link-active"> {{$route.name}} </a>
    <!-- Navbar links -->
    <b-navbar-nav class="align-items-center ml-md-auto">
      <!-- This item dont have <b-nav-item> because item have data-action/data-target on tag <a>, wich we cant add -->
      <li class="nav-item d-sm-none">
        <a class="nav-link" href="#" data-action="search-show" data-target="#navbar-search-main">
          <i class="ni ni-zoom-split-in"></i>
        </a>
      </li>
    </b-navbar-nav>
  
    <b-navbar-nav class="align-items-center ml-auto ml-md-0">
        
      <base-dropdown menu-on-right
                     class="nav-item"
                     tag="li"
                     title-tag="a"
                     title-classes="nav-link pr-0">
        <a href="#" class="nav-link pr-0" @click.prevent slot="title-container">
          <b-media no-body class="align-items-center">
                  <span class="avatar avatar-sm rounded-circle">
                    <img alt="Image placeholder" src="https://secure.gravatar.com/avatar/39d5218c0db52fc8b11529916cdf298d?size=80">
                  </span>
            <b-media-body class="ml-2 d-none d-lg-block">
              <span class="mb-0 text-sm  font-weight-bold">{{ user.username }}</span>
            </b-media-body>
          </b-media>
        </a>

        <template>

          <b-dropdown-header class="noti-title">
            <h6 class="text-overflow m-0">Willkommen!</h6>
          </b-dropdown-header>
          <router-link to="/profile" custom v-slot="{ navigate }" role="menuitem">
                <li class="dropdown-item" @click="navigate" @keypress.enter="navigate" role="link"> <i class="ni ni-single-02"></i> <span>Mein Profil</span></li>
          </router-link>
           <router-link to="/profile" custom v-slot="{ navigate }" role="menuitem">
                <li class="dropdown-item" @click="navigate" @keypress.enter="navigate" role="link"> <i class="ni ni-lock-circle-open"></i> <span>Passwort ändern</span></li>
          </router-link>
           <router-link to="/charge" custom v-slot="{ navigate }" role="menuitem">
                <li class="dropdown-item" @click="navigate" @keypress.enter="navigate" role="link"> <i class="ni ni-money-coins"></i> <span>Guthaben aufladen</span></li>
          </router-link>
           <router-link to="/ticket" custom v-slot="{ navigate }" role="menuitem">
                <li class="dropdown-item" @click="navigate" @keypress.enter="navigate" role="link"> <i class="ni ni-support-16"></i> <span>Deine Tickets</span></li>
          </router-link>
          <div class="dropdown-divider"></div>
                <li class="dropdown-item" @click="$store.dispatch('auth/logout') && $awn.success('Du wurdest abgemeldet!')" role="link"> <i class="ni ni-user-run"></i> <span>Ausloggen</span></li>

        </template>
        
      </base-dropdown>
    </b-navbar-nav>
  </base-nav>
  
</template>
<script>
import { CollapseTransition } from 'vue2-transitions';
import { BaseNav, Modal } from '@/components';
import { mapGetters } from 'vuex';

export default {
  components: {
    CollapseTransition,
    BaseNav,
    Modal,
  },
  props: {
    type: {
      type: String,
      default: 'default', // default|light
      description: 'Look of the dashboard navbar. Default (Green) or light (gray)'
    }
  },
  computed: {
    routeName() {
      const { name } = this.$route;
      return this.capitalizeFirstLetter(name);
    },
    ...mapGetters({ user: "auth/getUser" })
  },
  data() {
    return {
      activeNotifications: false,
      showMenu: false,
      searchModalVisible: false,
      searchQuery: ''
    };
  },
  methods: {
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    toggleNotificationDropDown() {
      this.activeNotifications = !this.activeNotifications;
    },
    closeDropDown() {
      this.activeNotifications = false;
    }
  }
};
</script>

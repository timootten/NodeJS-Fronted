<template>
  <div>
    <!-- Header -->
    <div class="header bg-gradient-success py-7 py-lg-8 pt-lg-9">
      <b-container>
        <div class="header-body text-center mb-7">
          <b-row class="justify-content-center">
            <b-col xl="5" lg="6" md="8" class="px-5">
              <h1 class="text-white">Passwort zurücksetzen!</h1>
              <p class="text-lead text-white">Setze schnell und einfach dein Passwort mit deiner E-Mail zurück.</p>
            </b-col>
          </b-row>
        </div>
      </b-container>
      <div class="separator separator-bottom separator-skew zindex-100">
        <svg x="0" y="0" viewBox="0 0 2560 100" preserveAspectRatio="none" version="1.1"
             xmlns="http://www.w3.org/2000/svg">
          <polygon class="fill-default" points="2560 0 2560 100 0 100"></polygon>
        </svg>
      </div>
    </div>
    <!-- Page content -->
    <b-container class="mt--8 pb-5">
      <b-row class="justify-content-center">
        <b-col lg="5" md="7">
          <b-card no-body class="bg-secondary border-0 mb-0">
            <b-card-header class="bg-transparent pb-5"  >
              <div class="text-muted text-center mt-2 mb-3"><small>Einloggen mit</small></div>
              <div class="btn-wrapper text-center">
                <!--
                <a href="#" class="btn btn-neutral btn-icon">
                  <span class="btn-inner--icon"><img src="img/icons/common/github.svg"></span>
                  <span class="btn-inner--text">Github</span>
                </a>-->
                <a :href="googleurl" class="btn btn-neutral btn-icon">
                  <span class="btn-inner--icon"><img src="img/icons/common/google.svg"></span>
                  <span class="btn-inner--text">Google</span>
                </a>
              </div>
            </b-card-header>
            <b-card-body class="px-lg-5 py-lg-5">
              <div class="text-center text-muted mb-4">
                <small>Passwort zurücksetzen</small>
              </div>
              <validation-observer v-slot="{handleSubmit}" ref="formValidator">
                <b-form role="form" @submit.prevent="handleSubmit(resetPassword)">
                  <base-input alternative
                              class="mb-3"
                              name="E-Mail"
                              :rules="{required: true, email: true}"
                              prepend-icon="ni ni-email-83"
                              placeholder="E-Mail"
                              v-model="email">
                  </base-input>

                  <div class="text-center">
                    <base-button type="primary" native-type="submit" class="my-4">Passwort zurücksetzen</base-button>
                  </div>
                </b-form>
              </validation-observer>
            </b-card-body>
          </b-card>
          <b-row class="mt-3">
            <b-col cols="6">
              <router-link to="/login" class="text-light"><small>Du willst dich anmelden?</small></router-link>
            </b-col>
            <b-col cols="6" class="text-right">
              <router-link to="/register" class="text-light"><small>Du hast noch keinen Account?</small></router-link>
            </b-col>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import AuthService from '../../services/AuthService'

export default {
    data() {
      return {
          email: '',
          googleurl: '#',
      };
    },
    created: function() {
      AuthService.getGoogleURL().then(response => this.googleurl = response.url);
    },
    methods: {
     async resetPassword() {
          const credentials = {
            email: this.email,
          };
          let notifier = this.$awn;
          notifier.async(
            AuthService.resetPassword(credentials),
            response =>  notifier.success(response.message), 
            error => notifier.alert(error.response.data.response.message),
            'Bitte warten'
          );
      }
    }
  };
</script>

<template>
  <div class="main-content bg-default">
    <base-nav
      v-model="showMenu"
      :transparent="true"
      menu-classes="justify-content-end"
      class="navbar-horizontal navbar-main navbar-top navbar-dark"
      expand="lg"
    >
      <div slot="brand" class="navbar-wrapper">
         <router-link class="navbar-brand active" to="/" custom v-slot="{ navigate }">
            <a href="/"><img style="width: 100px; height: auto;" @click="navigate" @keypress.enter="navigate" role="link" src="img/brand/green.png"></a>
        </router-link>
      </div>

     <template>
       <div class="navbar-collapse-header">
         <b-row>
           <b-col cols="6" class="collapse-brand">
             <router-link to="/" custom v-slot="{ navigate }">
               <a href="/"><img @click="navigate" @keypress.enter="navigate" role="link" src="img/brand/green.png"></a>
             </router-link>
           </b-col>
           <b-col cols="6" class="collapse-close">
             <button type="button" class="navbar-toggler" @click="showMenu = false">
               <span></span>
               <span></span>
             </button>
           </b-col>
         </b-row>
       </div>
         <b-navbar-nav  class="align-items-lg-center ml-lg-auto">
           <li class="nav-item">
            <router-link  class="nav-link" to="/register" custom v-slot="{ navigate }">
                <span><i class="ni ni-circle-08"/> <a href="" class="font-weight-bold text-white mt-5" @click="navigate" @keypress.enter="navigate" role="link">Registieren</a></span>
            </router-link>
           </li>
           <li class="nav-item">
            <router-link  class="nav-link" to="/login" custom v-slot="{ navigate }">
                <span><i class="ni ni-key-25"/> <a href="" class="font-weight-bold text-white mt-5" @click="navigate" @keypress.enter="navigate" role="link">Einloggen</a></span>
            </router-link>
           </li>
       </b-navbar-nav>
     </template>
    </base-nav>

    <div class="main-content">
      <zoom-center-transition
        :duration="pageTransitionDuration"
        mode="out-in"
      >
        <router-view></router-view>
      </zoom-center-transition>
    </div>

    <footer class="py-5" id="footer-main">
      <b-container >
        <b-row align-v="center" class="justify-content-xl-between">
          <b-col xl="6">
            <div class="copyright text-center text-xl-left text-muted">
              © {{year}} <a href="" class="font-weight-bold ml-1">ShadeHost.eu</a>
            </div>
          </b-col>
          <b-col xl="6" class="col-xl-6">
            <b-nav  class="nav-footer justify-content-center justify-content-xl-end">
              <sidebar-item
                :link="{
                name: 'AGB',
                path: '/agb',
                }"/>
              <sidebar-item
                :link="{
                name: 'Impressum',
                path: '/impressum',
                }"/>
              <sidebar-item
                :link="{
                name: 'Datenschutzerklärung',
                path: '/datenschutz',
                }"/>
              <sidebar-item
                :link="{
                name: 'Widerrufsbelehrung',
                path: '/wiederruf',
                }"/>
            </b-nav>
          </b-col>
        </b-row>
      </b-container>
    </footer>
  </div>
</template>
<script>
  import { BaseNav } from '@/components';
  import { ZoomCenterTransition } from 'vue2-transitions';

  export default {
    components: {
      BaseNav,
      ZoomCenterTransition
    },
    props: {
      backgroundColor: {
        type: String,
        default: 'black'
      }
    },
    data() {
      return {
        showMenu: false,
        menuTransitionDuration: 250,
        pageTransitionDuration: 200,
        year: new Date().getFullYear(),
        pageClass: 'login-page'
      };
    },
    computed: {
      title() {
        return `${this.$route.name} Page`;
      }
    },
    methods: {
      toggleNavbar() {
        document.body.classList.toggle('nav-open');
        this.showMenu = !this.showMenu;
      },
      closeMenu() {
        document.body.classList.remove('nav-open');
        this.showMenu = false;
      },
      setBackgroundColor() {
        document.body.classList.add('bg-default');
      },
      removeBackgroundColor() {
        document.body.classList.remove('bg-default');
      },
      updateBackground() {
        if (!this.$route.meta.noBodyBackground) {
          this.setBackgroundColor();
        } else {
          this.removeBackgroundColor()
        }
      }
    },
    beforeDestroy() {
      this.removeBackgroundColor();
    },
    beforeRouteUpdate(to, from, next) {
      // Close the mobile menu first then transition to next page
      if (this.showMenu) {
        this.closeMenu();
        setTimeout(() => {
          next();
        }, this.menuTransitionDuration);
      } else {
        next();
      }
    },
    watch: {
      $route: {
        immediate: true,
        handler: function () {
          this.updateBackground()
        }
      }
    }
  };
</script>
<style lang="scss">
  $scaleSize: 0.8;
  @keyframes zoomIn8 {
    from {
      opacity: 0;
      transform: scale3d($scaleSize, $scaleSize, $scaleSize);
    }
    100% {
      opacity: 1;
    }
  }

  .main-content .zoomIn {
    animation-name: zoomIn8;
  }

  @keyframes zoomOut8 {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
      transform: scale3d($scaleSize, $scaleSize, $scaleSize);
    }
  }

  .main-content .zoomOut {
    animation-name: zoomOut8;
  }
</style>

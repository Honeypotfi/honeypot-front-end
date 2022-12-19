<template>
  <v-app id="layout" class="relative">
    <Alerts ref="alerts" />
    <ModalsConnect ref="connect" />
    <Navbar ref="navbar" />
    <ModalsSwapModal ref="swap" />
    
    <v-main :class="wrapperSpace?'with':'without'" class="parent">
      <v-btn
        v-show="!$route.path.includes('/swap')" id="swap-floating-button" class="btn" @click="$refs.swap.modalSwap = true"
        @mousedown="dragFloatingBtn($event)" @touchstart="dragFloatingBtn($event)">
        <v-icon>mdi-chevron-up</v-icon>
        <span>Swap</span>
      </v-btn>

      <!-- <v-btn
        v-if="isLogged" id="account-floating-button" class="btn"
        @mousedown="dragFloatingBtn($event)" @touchstart="dragFloatingBtn($event)">
        {{user.accountId}}
        <v-icon>mdi-chevron-down</v-icon>
      </v-btn> -->

      <nuxt-child />
    </v-main>
    <!-- <Footer ref="footer"></Footer> -->
  </v-app>
</template>

<script>
// import computeds from '~/mixins/computeds'

export default {
  name: "DefaultLayout",
  // mixins: [computeds],
  // middleware: ['authenticated'],
  data() {
    return {
      wrapperSpace: true,
    }
  },
  created() {
    // get data profile
    // this.$store.dispatch("getData");
  },
  mounted() {
    this.scrollX();
    // this.footerHeightListener();
    
    // resize listener
    // window.addEventListener("resize", this.footerHeightListener);
  },
  beforeDestroy() {
    // window.removeEventListener("resize", this.footerHeightListener);
  },
  methods: {
    scrollX() {
      const
        scrollableDesktop = document.querySelectorAll('.scrollx'),
        scrollableMobile = document.querySelectorAll('.scrollxmobile');
      if (scrollableDesktop) {
        for (const el of scrollableDesktop) {
          el.addEventListener("wheel", event => { event.preventDefault(); el.scrollLeft += event.deltaY })
        }
      }
      if (scrollableMobile) {
        for (const el of scrollableMobile) {
          el.addEventListener("wheel", event => {
            if (window.innerWidth <= 880) { event.preventDefault(); el.scrollLeft += event.deltaY }
          })
        }
      }
    },
    // footerHeightListener() {
    //   setTimeout(() => {
    //     const footer = document.querySelector('#footer');
    //     document.documentElement.style.setProperty(
    //       '--h-footer', `${footer.getBoundingClientRect().height}px`
    //     );
    //   }, 400);
    // },
    dragFloatingBtn(event) {
      const target = event.currentTarget;
      let offset = [0,0], isDown = false;
      
      document.documentElement.style.overflow = "hidden"
      isDown = true;
      offset = [
        target.offsetTop - typeEvent(event).clientY
      ];
      
      function typeEvent(event) {
        if (event.type.includes('mouse')) {
          return event
        } else if (event.type.includes('touch')) {
          return event.touches[0]
        }
      }
      
      const onMove = (e) => {
        const
          position = typeEvent(e).clientY + offset[0],
          range = 100;
        if (e.type.includes('mouse')) e.preventDefault();
        if (isDown && position > range && position < window.innerHeight - (range + 80)) target.style.top  = `${position}px`;
      }
      const removeHandlers = () => {
        window.onmouseup = null
        window.onmousemove = null
        window.ontouchend = null
        window.ontouchmove = null
        isDown = false
        document.documentElement.style.overflow = "auto"
      }
      
      // desktop
      window.onmouseup = () => removeHandlers()
      window.onmousemove = e => onMove(e)
      // mobile
      window.ontouchend = () => removeHandlers()
      window.ontouchmove = e => onMove(e)
    }
  },
}
</script>

<style src="~/assets/styles/layouts/default.scss" lang="scss" />

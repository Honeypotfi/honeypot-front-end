<template>
  <div id="menuNavbar">
    <!--////////////// drawers //////////////-->
    <CustomeDrawer :model="drawer" @update="drawer = $event">
      <template #header>
        <a class="center" @click="$router.push(localePath('/')); $scrollTo('home')">
          <img src="~/assets/sources/logos/logo.svg" alt="logo" style="--w: 100%">
        </a>
        
        <!-- connect button -->
        <!-- <v-menu bottom offset-y nudge-bottom="10px">
          <template #activator="{ on, attrs }">
            <v-btn
              :class="$parent.isLogged ? 'btn2' : 'btn'"
              :style="$parent.isLogged ? '--bg: var(--accent)' : '--w: 75%; --min-h: 30px; --p: .5em 2em'"
              v-bind="$parent.isLogged ? attrs : ''"
              v-on="$parent.isLogged ? on : ''"
              @click="!$parent.isLogged ? $store.dispatch('modalConnect') : ''">
              <template v-if="$parent.isLogged">
                <span>{{$parent.user.accountId}}</span>
                <v-icon>mdi-chevron-down</v-icon>
              </template>
              
              <template v-else>Connect</template>
            </v-btn>
          </template>

          <v-list color="var(--accent)" style="--c:#fff">
            <v-list-item
              v-for="(item,i) in $parent.dataMenuLogin" :key="i"
              @click="item.key==='logout' ? $store.commit('signOut') : $router.push(localePath(key))">
              <v-list-item-title>{{item.name}}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu> -->
      </template>


      <template #content>
        <v-expansion-panels focusable accordion class="anim_moveleft">
          <v-expansion-panel
            v-for="(item, i) in $parent.dataNavbar" :key="i"
            @click="item.name !== 'portfolio' ? $router.push(localePath(item.to)) : ''"
          >
            <v-expansion-panel-header class="h10_em" expand-icon="mdi-menu-down" :hide-actions="item.to ? true : false">
              {{ item.name }}
            </v-expansion-panel-header>

            <v-expansion-panel-content v-if="!item.to">
              <v-list>
                <v-list-item
                  v-for="(item2,i2) in item.selection" :key="i2" :ripple="false"
                  :to="localePath(item2.to)" @click="drawer = false"
                >
                  <v-list-item-title class="center h10_em">
                    <span>{{ item2.name}}</span>
                  </v-list-item-title>
                </v-list-item>
              </v-list>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </template>


      <template #footer>
        <span class="h10_em clr_inv">Join us on:</span>

        <div class="center">
          <v-btn v-for="(item,i) in dataSocial" :key="i" icon :href="item.url" target="_blank">
            <img :src="require(`~/assets/sources/icons/${item.icon}.svg`)" alt="social red">
          </v-btn>
        </div>
      </template>
    </CustomeDrawer>
  </div>
</template>

<script>
export default {
  name: "NavbarMenuComponent",
  data() {
    return {
      drawer: false,
      dataSocial: [
        { icon:"twitter", url:"#" },
        { icon:"instagram", url:"#" },
        { icon:"twitch", url:"#" }
      ],
    };
  },
  methods: {
  },
};
</script>

<style src="~/assets/styles/components/navbar.scss" lang="scss" />

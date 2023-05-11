<template>
  <v-navigation-drawer v-model="drawer" fixed app overlay-color="green" :mini-variant.sync="mini" mini-variant-width.sync="80px"
                       permanent
  >

    <v-img contain :src="logo"  :aspect-ratio="mini ? 1 : 3" class="ma-4">

    </v-img>
    <v-list rounded dense>
      <!--      <NavigationItems-->
      <!--        v-for="(item, i) in navItems"-->
      <!--        :items="item.items"-->
      <!--        :title="item.title"-->
      <!--        :path="item.to"-->
      <!--        :prepend-icon="item.icon"-->
      <!--        :key="i"-->
      <!--      />-->
      <v-list-item class="pr-2 pl-2">
        <v-icon class="pl-0">
          mdi-home
        </v-icon>

        <v-list-item-title class="pl-4">Dashboard</v-list-item-title>

        <v-btn
          icon
          @click.stop="mini = !mini"
        >
          <v-icon>mdi-menu-left</v-icon>
        </v-btn>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
import NavigationItems from "@/components/Common/NavigationItems";
import logo from 'static/logo.png'

export default {
  name: 'NavSidebar',
  components: {
    NavigationItems,
  },

  data() {
    return {
      drawer: true,
      logo,
      mini: true,
      navItems: [
        {title: 'Dashboard', icon: 'mdi-home', to: '/dashboard', items: null},
        // {
        //   title: 'Order',
        //   path: null,
        //   icon: 'mdi-google-maps',
        //   items: [
        //     {title: 'All', icon: 'mdi-pencil', to: '/order'},
        //     {title: 'Pending', icon: 'mdi-close', to: '/'},
        //     {title: 'Cancelled', icon: 'mdi-close', to: '/'},
        //   ]
        // },
      ]
    }
  },
  mounted() {
    this.$root.$on('openDrawer', () => {
      this.showDrawer()
    })
  },
  methods: {
    showDrawer() {
      this.drawer = true
    }
  }
}
</script>

<style lang="scss" scoped></style>

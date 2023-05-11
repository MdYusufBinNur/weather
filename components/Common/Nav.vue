<template>
  <v-app-bar
    absolute
    style="background-color: transparent"
    flat
    app
    class="mt-2"
    fixed
  >
    <v-app-bar-nav-icon v-if="bp.smAndDown" @click="openDrawer"></v-app-bar-nav-icon>

    <v-toolbar-items v-show="bp.mdAndUp">
    </v-toolbar-items>
    <v-spacer></v-spacer>
    <v-spacer></v-spacer>
    <v-menu offset-y left transition="slide-y-transition">
      <template v-slot:activator="{ on, attrs }">
        <v-btn text plain small rounded v-bind="attrs" class="px-0" v-on="on" style="background-color: transparent">
          <v-avatar size="30" v-show="bp.mdAndUp">
            <v-img :src="'https://cdn.vuetifyjs.com/images/john.png'"></v-img>
          </v-avatar>
          <v-list-item-content v-show="bp.mdAndUp" class="px-0">

            <span class="pa-0 px-2 caption text-left text-capitalize">
              {{ username }}
            </span>
          </v-list-item-content>
          <v-icon left>
            mdi-menu-down
          </v-icon>
        </v-btn>

      </template>
      <v-card style="border-radius: 10px" max-width="200px">
        <v-list dense>
          <v-list-item
            class="black--text small"
            dense
            selectable
          >
            <v-avatar size="40" v-show="bp.mdAndUp">
              <v-img :src="'https://cdn.vuetifyjs.com/images/john.png'"></v-img>
            </v-avatar>
            <v-list-item-content v-show="bp.mdAndUp">
              <v-card-subtitle class="pa-0 px-2">
                {{ username }}
              </v-card-subtitle>
              <span class="pa-0 px-2 caption">
                {{ userRole ? userRole : '-' }}
              </span>
            </v-list-item-content>

          </v-list-item>
          <v-divider/>
          <v-list-item dense @click="logout">
            <v-list-item-avatar size="20">
              <v-img :src="logoutIcon" contain/>
            </v-list-item-avatar>
            <v-list-item-title class="text-capitalize">
              <v-btn text small plain :loading="loading">
                logout
              </v-btn>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-card>

    </v-menu>
    <v-dialog
      v-model="dialog"
      max-width="650px"
    >
      <v-card flat light rounded>
        <v-row align="center" no-gutters>
          <v-col cols="8">
            <v-card-title class="py-3 pb-0 h_primary">
              <span class="kep_title">Notification</span>
            </v-card-title>
          </v-col>
          <v-col cols="4" class="pa-2 text-right">
            <v-switch
              v-model="switch1"
              flat
              color="info"
              label="Only show unread"
            ></v-switch>
          </v-col>
        </v-row>
        <v-divider/>
        <v-form ref="form">
          <v-row no-gutters class="pa-2">
            Notification
          </v-row>
          <v-divider class="mb-2"/>
          <v-row no-gutters class="pa-2">
            <v-col cols="12" align="right" class="pa-2 pt-0">

            </v-col>
          </v-row>
        </v-form>
      </v-card>

    </v-dialog>
  </v-app-bar>
</template>

<script>
import logoutIcon from 'static/icons/Logout.png';
import ac from 'static/icons/ac.png';

export default {
  name: "Nav",

  data() {
    return {
      loadingState: false,
      loading: false,
      show: false,
      ac,
      logoutIcon,
      profileImg: null,
      userRole: null,
      date: '',
      menu2: false,
      adminMenu: false,
      dialog: false,
      drawer: false,
      username: '',
      userImage: '',
      switch1: true
    }
  },
  created() {
    this.initUser()
  },
  computed: {},
  methods: {
    close() {
      this.dialog = false
    },
    initUser() {
      this.username = this.$auth.user.data.username
    },
    openDrawer() {
      this.$root.$emit('openDrawer')
    },
    logout() {
      this.loading = true
      this.$axios.get('/logout')
        .then((response) => {
          this.$auth.logout()
          this.$router.push('/auth')
        })
        .catch((error) => {
          this.$toast.error(error.response.data.message)
        })
        .finally(() => {
          this.loading = false
        })
    }
  },

}
</script>

<style scoped>
.radius {
  border-radius: 10px;
}
</style>

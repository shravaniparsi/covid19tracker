<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-view-dashboard</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>COVID-19 TRACKER</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
       <Table></Table>
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy; BY SHRAVANI PARSI</span>
    </v-footer>
  </v-app>
</template>

<script>
import Table from './Table';
  export default {
    components: {
     Table,
     },
    props: {
    },
    data: () => ({
      drawer: false,
    }),
    created () {
      this.$vuetify.theme.dark = true
    },
  }
</script>

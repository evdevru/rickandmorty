<template>
  <v-app dark>
    <v-navigation-drawer dense v-model="drawer" clipped fixed app>
      <v-list>
        <v-list-item :to="{ name: 'characters' }" router exact>
          <v-list-item-action>
            <v-icon>mdi-account-multiple</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="'Characters'" />
          </v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-action>
            <v-icon>mdi-map-marker-multiple</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="'Locations'" />
          </v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-action>
            <v-icon>mdi-account-multiple</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="'Episodes'" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <template #append>
        <div class="d-flex justify-space-around pt-3 pb-3">
          <v-tooltip top>
            <template #activator="{ on, attrs }">
              <v-icon v-bind="attrs" v-on="on">mdi-facebook</v-icon>
            </template>
            <span>Facebook</span>
          </v-tooltip>
          <v-tooltip top>
            <template #activator="{ on, attrs }">
              <v-icon v-bind="attrs" v-on="on">mdi-instagram</v-icon>
            </template>
            <span>Instagram</span>
          </v-tooltip>
          <v-tooltip top>
            <template #activator="{ on, attrs }">
              <v-icon v-bind="attrs" v-on="on">mdi-reddit</v-icon>
            </template>
            <span>Reddit</span>
          </v-tooltip>
        </div>
      </template>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="true" :fixed="false" app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-avatar color="accent" size="46"></v-avatar>
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      title: 'Rick and Morty',
    }
  },
}
</script>

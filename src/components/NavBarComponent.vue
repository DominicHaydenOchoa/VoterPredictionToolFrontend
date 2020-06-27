<template>
  <div class="dashboard">
      <v-navigation-drawer
          v-model="drawer"
          :color="color"
          floating
          :expand-on-hover="expandOnHover"
          :mini-variant="miniVariant"
          :right="right"
          :permanent="permanent"
          :src="bg"
          absolute
          dark
        >
          <v-list
            dense
            nav
            class="py-0"
          >
            <v-list-item two-line :class="miniVariant && 'px-0'">
              <v-list-item-avatar>
                <img src="https://i0.wp.com/www.cathwear.com/wp-content/uploads/2019/12/avatar-icon-scaled.png?fit=2560%2C2560&ssl=1">
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title>Hispanic Early Voting</v-list-item-title>
                <v-list-item-subtitle>Data & Prediction Tool</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list-item
              v-for="item in items"
              :key="item.title"
              link
            >
              <v-list-item-icon>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-icon>

              <v-list-item-content>
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  name: 'NavBarComponent',
  data: function () {
    return {
      sidebarMenu: true,
      toggleMini: true,
      drawer: true,
      items: [
        { title: 'Training Data Breakdown', icon: 'mdi-view-dashboard' },
        { title: 'Testing Data Breakdown', icon: 'mdi-view-dashboard' },
        { title: 'Vote Prediction Tool', icon: 'mdi-image' },
        { title: 'About', icon: 'mdi-help-box' }
      ],
      color: '',
      colors: [
        'primary',
        'blue',
        'success',
        'red',
        'teal'
      ],
      right: false,
      permanent: true,
      miniVariant: true,
      expandOnHover: true,
      background: true
    }
  },

  computed: {
    bg () {
      return this.background
        ? 'https://wallpaperaccess.com/full/99810.jpg' : undefined
    }
  }
}
</script>>

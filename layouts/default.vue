<template>
  <v-app>
    <!-- ツールバー -->
    <v-toolbar
      color="blue-grey"
      dark
      fixed
      app
      clipped-right
    >
      <!-- <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon> -->
      <v-toolbar-title v-text="title" />
      <!-- <v-spacer></v-spacer>
      <v-toolbar-side-icon @click.stop="drawerRight = !drawerRight"></v-toolbar-side-icon> -->
    </v-toolbar>

    <!-- コンテンツ -->
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>

  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawer: null,
    drawerRight: null,
    // right: false,
    // left: false,
    title: 'うなりざきメールテンプレ',
    valid: false,
    date: new Date().toISOString().substr(0, 10)
    // nameRules: [v => !!v || 'required']
  })
}
</script>

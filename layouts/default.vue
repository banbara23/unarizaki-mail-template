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
      <v-toolbar-title v-text="title" />
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
    title: 'うなりざきメールテンプレ',
    valid: false,
    date: new Date().toISOString().substr(0, 10)
  })
}
</script>

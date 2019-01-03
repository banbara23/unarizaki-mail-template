<template>
  <v-flex
    xs12
    sm12
    md12
  >
    <v-card>
      <v-card-title class="headline">{{title}}</v-card-title>
      <v-card-text>
        <pre>{{text}}</pre>
      </v-card-text>
      <v-card-actions>
        <v-spacer />
        <v-btn
          color="primary"
          flat
          nuxt
        >Copy to Clip</v-btn>
      </v-card-actions>
    </v-card>
  </v-flex>
</template>

<script>
export default {
  props: ['title', 'text', 'sender_name', 'checkin_date'],
  mounted: function() {
    this.text = this.text.replace('sender_name', this.sender_name)
    // this.$nextTick(function() {
    //   console.log(this.sender_name)
    //   // vm.text = vm.text.replace('sender_name', sender_name)
    // })
  },
  methods: {
    replaceText: function(text) {
      text.replace('sender_name', this.sender_name)
    }
  }
}
</script>
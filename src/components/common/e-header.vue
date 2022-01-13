<template>
  <v-app-bar app elevation="1" color="#fff">
    <e-wrap wrap-class="wrap-0" con-class="d-flex al-c">
      <a href="/" class="mr-2"
        ><img contain src="img/logo.png" height="40" class="d-b"
      /></a>
      <!-- <div class="bg-s1 d-flex al-c" :class="asMobile ? 'ml-auto' : 'ml-3'">
        <input type="text" />
        <v-icon color="#fff" size="20">mdi-magnify</v-icon>
      </div> -->
      <!-- <v-spacer></v-spacer> -->
      <!-- <div class="ml-auto">
        <v-btn text small> Connect Wallet </v-btn>
      </div> -->
    </e-wrap>
  </v-app-bar>
</template>

<script>
export default {
  computed: {
    asMobile() {
      return this.$vuetify.breakpoint.smAndDown;
    },
    path() {
      const { path } = this.$route || {};
      return path;
    },
  },
  data() {
    return {
      searchKey: "",
    };
  },
  methods: {},
};
</script>

<style lang="scss">
.bg-s1 {
  padding: 0 4px;
  background: #8ca1b6;
  input {
    outline: none;
    padding: 5px;
    color: #fff;
  }
}
</style>
<template>
  <div class="list-1">
    <!-- <v-tabs height="45">
      <v-tab>All</v-tab>
      <v-tab>New</v-tab>
    </v-tabs> -->
    <div class="mt-6">
      <v-skeleton-loader
        v-if="!list"
        type="article"
        max-width="500"
      ></v-skeleton-loader>

      <v-row v-else>
        <v-col cols="12" md="3" v-for="(it, i) in list" :key="i">
          <div class="item pa-4 pl-5 bdrs-3">
            <div class="d-flex">
              <v-avatar :size="55">
                <v-img :src="it.logo || `img/logo/${it.title}.png`"></v-img>
              </v-avatar>
              <div class="ml-auto ta-r">
                <v-chip small color="#A184BC" v-if="it.mainnet">
                  <span class="white-0">{{ it.mainnet }}</span>
                </v-chip>
                <div class="pa-1 pr-0">
                  <v-chip small color="#ECB176" v-if="it.type">
                    <span class="white-0">{{ it.type }}</span>
                  </v-chip>
                </div>
              </div>
            </div>
            <h3 class="mt-3 fz-15">{{ it.title }}</h3>
            <div class="mt-1 fz-13 gray-3 line-3" style="min-height: 60px">
              {{ it.desc }}
            </div>
            <div class="mt-2 link-wrap">
              <div>
                <v-btn
                  color="primary"
                  text
                  x-small
                  :href="it.link"
                  target="_blank"
                  v-if="it.link"
                >
                  <v-icon size="14">mdi-web</v-icon>
                  <span class="ml-1">{{
                    it.link
                      .replace("https://", "")
                      .replace(/\/$/, "")
                      .cutStr(0, 30)
                  }}</span>
                </v-btn>
              </div>
              <div>
                <v-btn
                  color="primary"
                  text
                  x-small
                  :href="it.git"
                  target="_blank"
                  v-if="it.git"
                >
                  <v-icon size="14">mdi-github</v-icon>
                  <span class="ml-1">{{
                    it.git.replace("https://github.com/", "").cutStr(0, 30)
                  }}</span>
                </v-btn>
              </div>
              <div>
                <v-btn
                  color="primary"
                  text
                  x-small
                  :href="it.hosting"
                  target="_blank"
                  v-if="it.hosting"
                >
                  <v-icon size="14">mdi-webpack</v-icon>
                  <span class="ml-1">{{
                    it.hosting
                      .replace("https://", "")
                      .replace(/\/$/, "")
                      .cutStr(0, 30)
                  }}</span>
                </v-btn>
              </div>
            </div>
          </div>
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    asMobile() {
      return this.$vuetify.breakpoint.smAndDown;
    },
  },
  data() {
    return {
      list: null,
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    async getList() {
      try {
        const { data } = await this.$http.get("/portal/list.json");
        // console.log(data);
        this.list = data;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style lang="scss">
.list-1 {
  .item {
    background: #edf2f7;
  }
  .link-wrap {
    margin-left: -10px;
    & > div {
      margin-top: 2px;
    }
  }
}
</style>
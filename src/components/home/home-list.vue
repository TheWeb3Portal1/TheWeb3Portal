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

      <div v-else>
        <div class="ta-c fw-b mt-15" v-if="!resList.length">
          No results Found. Try changing your filters or
          <a href="" target="_blank">submit an inclusion request</a>
        </div>
        <v-row>
          <v-col cols="12" md="3" v-for="(it, i) in resList" :key="i">
            <div class="item pa-4 pl-5 bdrs-3">
              <div class="d-flex">
                <a :href="it.link" target="_blank" class="hover-1">
                  <v-avatar :size="55">
                    <v-img :src="it.logo || `img/logo/${it.title}.png`"></v-img>
                  </v-avatar>
                </a>
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
              <h3 class="mt-3 fz-15">
                <a :href="it.link" target="_blank">{{ it.title }}</a>
              </h3>
              <div class="mt-1 fz-13 gray-3 line-3" style="min-height: 60px">
                {{ it.desc }}
              </div>
              <div class="mt-2 link-wrap">
                <div>
                  <v-btn
                    color="primary"
                    text
                    x-small
                    :href="it.hosting"
                    target="_blank"
                    v-if="it.hosting"
                  >
                    <v-icon size="14">mdi-web</v-icon>
                    <span class="ml-1">{{
                      it.hosting
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
                    :href="`https://ipfs.io/ipfs/${it.cid}`"
                    target="_blank"
                    v-if="it.cid"
                  >
                    <v-icon size="14">mdi-webpack</v-icon>
                    <span class="ml-1">ipfs/{{ it.cid.cutStr(10, 10) }}</span>
                  </v-btn>
                </div>
              </div>
            </div>
          </v-col>
        </v-row>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: {
    ...mapState({
      searchKey: (s) => s.searchKey,
    }),
    asMobile() {
      return this.$vuetify.breakpoint.smAndDown;
    },
    resList() {
      if (!this.searchKey) return this.list;
      return this.list.filter((it) => {
        const reg = new RegExp(this.searchKey, "im");
        return reg.test(it.title + it.desc);
      });
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
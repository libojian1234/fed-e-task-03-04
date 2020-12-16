<template>
  <Layout>
    <div>
      <el-card
        shadow="never"
        style="
          min-height: 400px;
          margin-bottom: 20px;
          padding: 0px 0px 20px 0px;
        "
      >
        <el-tabs type="card" v-model="activeTab">
          <el-tab-pane
            :label="'粉丝 ' + $page.followers.pageInfo.totalItems"
            name="followers"
            style="padding: 5px"
          >
            <div>
              <div v-if="followers.length">
                <el-row style="min-height: 200px">
                  <el-col
                    :span="8"
                    v-for="({ node: item }, index) in followers"
                    :key="'followers' + index"
                    style="padding: 10px"
                  >
                    <el-card
                      shadow="hover"
                      style="font-size: 13px; color: #606266; line-height: 20px"
                    >
                      <i class="el-icon-star-off"></i>&emsp;
                      <a
                        @click="
                          $router.push(`/user/social/details/${item.name}`)
                        "
                        style="text-decoration: none; cursor: pointer"
                        >{{ item.name }}</a
                      >
                      <br />
                      <i class="el-icon-message"></i>&emsp;
                      <a
                        :href="item.htmlUrl"
                        target="_blank"
                        style="text-decoration: none; cursor: pointer"
                        >TA的主页</a
                      >
                      <br />
                      <img
                        :src="GRIDSOME_API_URL + item.avatar.url"
                        style="width: 30px; border-radius: 5px; margin-top: 5px"
                      />
                    </el-card>
                  </el-col>
                </el-row>
                <Pager :info="$page.followers.pageInfo" />
              </div>
              <div
                style="
                  min-height: 300px;
                  margin-bottom: 20px;
                  padding: 20px 0px 20px 0px;
                  text-align: center;
                "
                v-else
              >
                <font style="font-size: 30px; color: #dddddd">
                  <b>(￢_￢) 没有一个粉丝</b>
                </font>
              </div>
            </div>
          </el-tab-pane>
          <el-tab-pane
            :label="'关注 ' + $page.following.pageInfo.totalItems"
            name="following"
            style="padding: 5px"
          >
            <div>
              <div v-if="following.length">
                <el-row style="min-height: 200px">
                  <el-col
                    :span="8"
                    v-for="({ node: item }, index) in following"
                    :key="'following' + index"
                    style="padding: 10px"
                  >
                    <el-card
                      shadow="hover"
                      style="font-size: 13px; color: #606266; line-height: 20px"
                    >
                      <i class="el-icon-star-off"></i>&emsp;
                      <a
                        @click="
                          $router.push(`/user/social/details/${item.name}`)
                        "
                        style="text-decoration: none; cursor: pointer"
                        >{{ item.name }}</a
                      >
                      <br />
                      <i class="el-icon-message"></i>&emsp;
                      <a
                        :href="item.htmlUrl"
                        target="_blank"
                        style="text-decoration: none; cursor: pointer"
                        >TA的主页</a
                      >
                      <br />
                      <img
                        :src="GRIDSOME_API_URL + item.avatar.url"
                        style="width: 30px; border-radius: 5px; margin-top: 5px"
                      />
                    </el-card>
                  </el-col>
                </el-row>
                <Pager :info="$page.following.pageInfo" />
              </div>
              <div
                style="
                  min-height: 300px;
                  margin-bottom: 20px;
                  padding: 20px 0px 20px 0px;
                  text-align: center;
                "
                v-else
              >
                <font style="font-size: 30px; color: #dddddd">
                  <b>(￢_￢) 还没有关注一个人</b>
                </font>
              </div>
            </div>
          </el-tab-pane>
        </el-tabs>
      </el-card>
    </div>
  </Layout>
</template>
<page-query>
query ($page: Int) {
  followers: allStrapiFollowers (perPage: 4, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
      totalItems
    }
    edges {
      node {
        id
        name
        htmlUrl
        avatar{
          url
        }
      }
    }
  },
  following: allStrapiFollowing (perPage: 3, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
      totalItems
    }
    edges {
      node {
        id
        name
        htmlUrl
        avatar{
          url
        }
      }
    }
  }
}
</page-query>
<script>
import { Pager } from "gridsome";
export default {
  components: {
    Pager,
  },
  data() {
    return {
      activeTab: "followers"
    };
  },
  computed: {
    followers() {
      return this.$page.followers.edges;
    },
    following() {
      return this.$page.following.edges;
    },
  },
  methods: {
    listFollowers() {
      this.followers.loading = true;
      UserApi.followers(this.followers.query)
        .then((response) => {
          let result = response.data;
          let pageNumber = this.$util.parseHeaders(response.headers);
          if (pageNumber) {
            this.followers.query.pageNumber = pageNumber;
          }
          this.followers.list = [];
          for (let i = 0; i < result.length; i++) {
            let data = {};
            data.name = result[i]["login"];
            data.avatarUrl = result[i]["avatar_url"];
            data.htmlUrl = result[i]["html_url"];
            this.followers.list.push(data);
          }
        })
        .then(() => (this.followers.loading = false));
    },
    listFollowing() {
      this.following.loading = true;
      UserApi.following(this.following.query)
        .then((response) => {
          let result = response.data;
          let pageNumber = this.$util.parseHeaders(response.headers);
          if (pageNumber) {
            this.following.query.pageNumber = pageNumber;
          }
          this.following.list = [];
          for (let i = 0; i < result.length; i++) {
            let data = {};
            data.name = result[i]["login"];
            data.avatarUrl = result[i]["avatar_url"];
            data.htmlUrl = result[i]["html_url"];
            this.following.list.push(data);
          }
        })
        .then(() => (this.following.loading = false));
    },
  },
};
</script>
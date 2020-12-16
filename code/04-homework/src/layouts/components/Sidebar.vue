<template>
  <div>
    <el-card shadow="never">
      <el-menu :default-active="active">
        <g-link
          v-for="item in constantRouterMap"
          :key="item.path"
          :to="item.path"
        >
          <el-menu-item :class="{ 'is-active': path === item.path }">
            <i :class="item.icon"></i>
            <span slot="title">{{ item.title }}</span>
          </el-menu-item>
        </g-link>
      </el-menu>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      constantRouterMap: [
        {
          path: "/",
          title: "最新动态",
          icon: "el-icon-star-off",
        },
        {
          path: "/social",
          title: "社交圈",
          icon: "el-icon-mobile-phone",
        },
        {
          path: "/blog",
          title: "博客列表",
          icon: "el-icon-edit-outline",
        }
      ],
      active: "",
      parentUrl: "",
      menuList: [],
    };
  },
  computed: {
    path() {
      return this.$route.path;
    },
  },
};
</script>
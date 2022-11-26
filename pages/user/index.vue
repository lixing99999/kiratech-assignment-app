<template>
  <div>
    <el-menu class="menu d-flex align-items-center" mode="horizontal">
      <el-menu-item class="mr-auto p2">
        <img id="logo" :src="require('~/assets/Logo.png')" />
      </el-menu-item>
      <el-menu-item class="p2">
        <img
          id="notification-icon"
          class="icon"
          :src="require('~/assets/bell.png')"
        />
      </el-menu-item>
      <el-menu-item class="p2">
        <img
          id="setting-icon"
          class="icon"
          :src="require('~/assets/settings.png')"
        />
      </el-menu-item>
      <el-menu-item class="p2">
        <img
          id="logout-icon"
          class="icon"
          :src="require('~/assets/logout.png')"
        />
      </el-menu-item>
    </el-menu>
    <user-header></user-header>
    <div class="mt-5 mx-5">
      <user-table></user-table>
    </div>
  </div>
</template>

<script>
import UserHeader from "../../components/user/header.vue";
import UserTable from "../../components/user/table.vue";
export default {
  components: {
    UserHeader,
    UserTable,
  },
  head() {
    return {
      title: "Users",
    };
  },
  data() {
    return {
      activeIndex: "1",
      activeIndex2: "1",
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
  },
};
</script>

<style scoped>
#logo {
  height: 3.5rem;
}

.icon {
  height: 1.5rem;
}

.menu {
  height: 5rem;
}
</style>

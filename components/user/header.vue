<template>
  <div id="user-header" class="d-flex">
    <div id="avatar" class="p2">
      <el-avatar
        shape="square"
        :size="150"
        :src="require('~/assets/Avatar.png')"
      ></el-avatar>
    </div>
    <div class="ml-4 pb-4 align-self-end">
      <div id="" class="user-header-text h1">{{ user.name }}</div>
      <div id="" class="user-header-text p">
        last online : {{ user.last_online_datetime_utc }}
      </div>
    </div>
    <div class="ml-4 pb-4 align-self-end">
      <el-button class="user-header-button" icon="el-icon-s-promotion"
        >Send Message</el-button
      >
      <el-button class="user-header-button" icon="el-icon-plus"
        >Add Friend</el-button
      >
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        name: "John Doe",
        last_online_datetime_utc: "2 days ago",
      },
    };
  },
};
</script>

<style scoped>
#user-header {
  height: 10rem;
  background: #35bad8;
  padding-left: 12em;
}

#avatar {
  position: relative;
  bottom: -2.5em;
}

.user-header-text {
  color: white;
}

.user-header-button {
  background: #35bad8;
  color: white;
  border-color: white;
  height: 50px;
}

.user-header-button:hover {
  background: white;
  color: #35bad8;
  border-color: #35bad8;
}
</style>

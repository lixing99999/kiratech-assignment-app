<template>
  <div class="mx-5" v-loading="loading">
    <el-input
      placeholder="Enter user email"
      prefix-icon="el-icon-search"
      v-model="input"
      @change="search"
    >
    </el-input>
    <div class="d-flex p-2">
      <div class="p-2 user-table-title" @click="sort('date')">
        <i :class="order ? 'el-icon-sort-down' : 'el-icon-sort-up'" /> Date
      </div>
      <div
        class="p-2 user-table-title"
        style="width: 30%"
        @click="sort('name.first')"
      >
        <i :class="!order ? 'el-icon-sort-down' : 'el-icon-sort-up'" />
        Name
      </div>
      <div class="p-2 user-table-title" @click="sort('gender')">
        <i :class="order ? 'el-icon-sort-down' : 'el-icon-sort-up'" /> Gender
      </div>
      <div class="p-2 user-table-title" @click="sort('location.country')">
        <i :class="order ? 'el-icon-sort-down' : 'el-icon-sort-up'" />
        Country
      </div>
      <div class="p-2 user-table-title" @click="sort('email')">
        <i :class="order ? 'el-icon-sort-down' : 'el-icon-sort-up'" /> Email
      </div>
    </div>
    <div
      class="d-flex mb-2 user-table-row p-2 border rounded"
      @click="handleUserDetailDialog(user)"
      v-for="(user, idx) in users"
      :key="idx"
    >
      <div class="p-2 align-self-center">
        {{ _moment(user.registered.date).format("DD MMM YYYY") }}
      </div>
      <div class="p-2 align-self-center user-table-row-name" style="width: 30%">
        {{ user.name.first }} {{ user.name.last }}
      </div>
      <div class="p-2 align-self-center">
        {{ user.gender }}
      </div>
      <div class="p-2 align-self-center user-table-row-country">
        {{ user.location.country }}
      </div>
      <div class="p-2 align-self-center">
        {{ user.email }}
      </div>
    </div>
    <div class="d-flex justify-content-end mt-3" style="margin: -4px">
      <el-pagination
        background
        layout="prev, pager, next"
        @current-change="getUsers"
        :page-size="10"
        :total="100"
      >
      </el-pagination>
    </div>
    <el-dialog :show-close="false" :visible.sync="userDetailDialog" width="40%">
      <div class="px-3 d-flex justify-content-between">
        <div class="user-detail-dialog-name">
          {{ user.name.first }} {{ user.name.last }}
        </div>
        <i
          size="medium"
          class="el-icon-close"
          @click="userDetailDialog = !userDetailDialog"
        ></i>
      </div>
      <div class="p-3">
        <div class="d-flex">
          <div class="user-detail-dialog-title">Date :</div>
          <div class="user-detail-dialog-data">{{ user.registered.date }}</div>
        </div>
        <div class="d-flex">
          <div class="user-detail-dialog-title">Status :</div>
          <div class="user-detail-dialog-data">{{ user.status }}</div>
        </div>
        <div class="d-flex">
          <div class="user-detail-dialog-title">Gender :</div>
          <div class="user-detail-dialog-data">{{ user.gender }}</div>
        </div>
        <div class="d-flex">
          <div class="user-detail-dialog-title">Country :</div>
          <div class="user-detail-dialog-data">{{ user.location.country }}</div>
        </div>
        <div class="d-flex">
          <div class="user-detail-dialog-title">Email :</div>
          <div class="user-detail-dialog-data">{{ user.email }}</div>
        </div>
      </div>
    </el-dialog>
    <div class="d-flex justify-content-center my-5" @click="getUsers">
      <el-button type="primary" icon="el-icon-refresh"></el-button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import UserDetail from "../../components/user/detail.vue";
import _ from "lodash";
export default {
  components: {
    UserDetail,
  },
  data() {
    return {
      users: [],
      userDetailDialog: false,
      user: {
        name: {
          first: "",
          last: "",
        },
        registered: {
          date: "",
        },
        gender: "",
        location: {
          country: "",
        },
        email: "",
      },
      loading: false,
      total: 100,
      page: 1,
      order: true,
      input: "",
    };
  },
  created() {
    this.getUsers();
  },
  methods: {
    _moment: (date) => {
      return moment(date);
    },
    async getUsers(page = 1) {
      this.loading = true;
      this.page = page;
      const result = await axios(
        `https://randomuser.me/api/?page=${this.page}&results=10&gender=${this.input}&seed=kiratech-001`
      );
      this.users = result.data.results;
      this.loading = false;
    },
    handleUserDetailDialog(user) {
      this.user = user;
      this.userDetailDialog = !this.userDetailDialog;
    },
    sort(key) {
      this.users = _.orderBy(this.users, key, this.order ? "asc" : "desc");
      this.order = !this.order;
    },
    async search(key) {
      await this.getUsers(1);
      console.log(this.users);

      this.users = _.filter(this.users, (x) => {
        console.log(x.name.first.indexOf(key) >= 0);
        return x.email.indexOf(key) >= 0;
      });
    },
  },
};
</script>

<style>
.user-table-row {
  height: 5rem;
  box-shadow: 0px 5px 5px 0px lightgrey;
  font-size: 0.8rem;
  color: #bcbcbc;
}

.user-table-row > div {
  width: 20%;
}

.user-table-row:hover {
  border: 2px solid #35bad8 !important;
}

.user-table-row:hover > .user-table-row-name {
  color: #35bad8 !important;
}

.user-table-title {
  color: #bcbcbc;
  font-size: 0.8rem;
  width: 20%;
}

.user-table-row-name {
  font-weight: bold;
  color: black;
}

.user-table-row-country {
  color: black;
}

.user-detail-dialog-name {
  font-size: 2rem;
  font-weight: bold;
  color: black;
}

.user-detail-dialog-title {
  color: #bcbcbc;
  font-size: 1rem;
  width: 20%;
  margin-top: 10px;
}

.user-detail-dialog-data {
  color: black;
  font-size: 1rem;
  width: 80%;
  margin-top: 10px;
}

.el-dialog__header {
  padding: 0 !important;
}
</style>

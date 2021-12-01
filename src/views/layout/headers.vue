<template>
  <div style="font-weight:bold">
    <el-row class="tac">
      <el-col :span="24">
        <el-menu  default-active="1-1" @open="handleOpen" @close="handleClose"  background-color="transparent"  active-text-color="#cdaeff"  text-color="#fff" > 
          
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>区块链信息</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="1-1" @click="routerLink('home')">首页</el-menu-item>
              <el-menu-item index="1-2" @click="routerLink('block')">区块</el-menu-item>
              <el-menu-item index="1-3" @click="routerLink('transaction')">交易</el-menu-item>
            </el-menu-item-group>
          </el-submenu>

          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-s-tools"></i>
              <span>配置信息</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="2-1" @click="routerLink('groupConfig')">群组配置</el-menu-item>
              <el-menu-item index="2-2" @click="routerLink('nodeConfig')">节点配置</el-menu-item>
            </el-menu-item-group>
          </el-submenu>

          <el-submenu index="3">
            <template slot="title">
              <i class="el-icon-menu"></i>
              <span>图片信息</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="3-1" @click="routerLink('pictureHome')">首页</el-menu-item>
              <!-- <el-menu-item index="2-2">交易图片总量</el-menu-item>
              <el-menu-item index="2-3">侵权图片交易</el-menu-item> -->
            </el-menu-item-group>
          </el-submenu>
        </el-menu> 
      </el-col>
    </el-row>
  </div>
</template>

<script type="es6">
import url from "@/api/url";
import router from "@/router";
import constant from "@/util/constant";
import { message } from "@/util/util";
import Bus from "@/bus";

export default {
  name: "headers",
  data: function() {
    return {
      menu: constant.MENU_LIST,
      groupList: [],
      groupName: "",
      chainNone: false,
      groupId: ""
    };
  },
  mounted: function() {
    this.$nextTick(function() {
      this.getGroupData();
    });
    Bus.$on("change", data => {
      this.getGroupData();
    });
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log("打开了");
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    // get groups
    getGroupData: function() {
      this.groupList = JSON.parse(localStorage.getItem("groupList"));
      if (this.groupList.length) {
        this.groupId = JSON.parse(localStorage.getItem("groupId"));
        this.groupList.forEach(value => {
          if (value.groupId == this.groupId) {
            this.groupName = value.groupName;
          }
        });
      }
    },
    //Switching group
    checkGroup: function(val) {
      this.groupName = val.groupName;
      this.groupId = val.groupId;
      localStorage.setItem("groupId", this.groupId);
      console.log(this.$route.path);
      console.log(this.$route);
      router.push({
        name: "blankPage",
        query: {
          path: this.$route.path
        }
      });
    },
    routerLink: function(name) {
      console.log("123");
      router.push({
        name: name
      });
    },
    link: function() {
      router.push({
        name: "home"
      });
    }
  }
};
</script>
<style>

</style>

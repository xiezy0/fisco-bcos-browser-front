<template>
  <div style="height:100%;" class="main-all">
    <div
      v-loading="loading"
      element-loading-text="数据加载中..."
      element-loading-background="rgba(0, 0, 0, 0.8)"
    
    >
      <el-container>
        <el-aside width="250px">
          <v-head ref="head"></v-head>
        </el-aside>
        <el-main >
          <router-view v-if="routerShow" class="main" @addGroup="change" ></router-view>
        </el-main>
      </el-container>
    </div>
    <add-group @close="addGroups" v-if="addGroupShow" @success="addSuccess" :show="addGroupShow"></add-group>
  </div>
</template>
<script>
import headers from "./headers";
import addGroup from "@/components/addGroup";
import { getGroupList } from "@/api/api";
import "@/assets/css/layout.css";
import "@/assets/css/public.css";
import url from "@/api/url";
import router from "@/router";
import constant from "@/util/constant";
import { message } from "@/util/util";

export default {
  name: "mains",
  components: {
    "v-head": headers,
    "add-group": addGroup
  },
  data: function() {
    return {
      loading: false,
      addGroupShow: false,
      groupList: [],
      groupId: null,
      routerShow: false
    };
  },
  mounted: function() {
    this.$nextTick(function() {
      this.GetgroupList();
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
    change: function() {
      this.$refs.head.getGroupData();
    },
    GetgroupList: function() {
      let data = {};
      getGroupList(data)
        .then(res => {
          if (res.data.code === 0) {
            this.routerShow = true;
            if (res.data.data.length) {
              this.groupList = res.data.data;
              if (!localStorage.getItem("groupId")) {
                this.groupId = res.data.data[0].groupId;
                localStorage.setItem("groupId", this.groupId);
              }
              localStorage.setItem("groupList", JSON.stringify(this.groupList));
              this.change();
              if (this.$route.query.pkHash) {
                router.push({
                  name: this.$route.query.path,
                  query: {
                    pkHash: this.$route.query.pkHash
                  }
                });
              } else if (this.$route.query.blockHash) {
                router.push({
                  name: this.$route.query.path,
                  query: {
                    blockHash: this.$route.query.blockHash
                  }
                });
              } else {
                router.push({
                  name: this.$route.query.path
                });
              }
            } else {
              router.push({
                name: "groupConfig"
              });
            }
          } else {
            message(res.data.message, "error");
          }
        })
        .catch(err => {
          message(constant.ERROR, "error");
        });
    }
  }
};
</script>
<style>
.el-aside {
  color: #333;
 
  background-color: #01233a;
  background-image: url("../../assets/images/barBackground.png");
  background-size: 100%;
  background-repeat: no-repeat;
}

.el-main {
  min-height:700px;
  
  padding:0px;
}
.main-all {
  overflow: scroll;
}
.main {
  width: 100%;
  background-color: #2a2c3b;
}
.el-message__content {
  display: inline-block;
}
.el-message__closeBtn {
  display: inline-block !important;
  vertical-align: middle !important;
  line-height: 0 !important;
}
</style>

<template>
  <div class="index-header">
    <!--    顶栏

    1.登陸問題-->
    <nav class="navbar index-header-bar">
      <el-row>
        <el-col :span="3" :offset="3">
          <span>惠工校园二手交易平台</span>
        </el-col>
        <el-col :span="10" :offset="5">
          <el-row>
            <el-col :span="3" :offset="1" class="index-header-bar-span">
              <router-link to="/">首页</router-link>
            </el-col>
            <el-col :span="3" :offset="1" class="index-header-bar-span">
              <a href="#" data-toggle="dropdown">{{userName}}</a>
              <ul class="dropdown-menu my-dropdown-menu-1">
                <li>
                  <a class="my-dropdown-menu-li-1" @click="checkLogin1" v-show="userName==='登录/注册'">登录</a>
                </li>
                <li>
                  <a class="my-dropdown-menu-li-1" @click="checkLogin2" v-show="userName==='登录/注册'">注册</a>
                </li>
                <li>
                  <a class="my-dropdown-menu-li-1" v-show="userName!=='登录/注册'" @click="exit()">退出</a>
                </li>
              </ul>
            </el-col>
            <el-col :span="3" :offset="1" class="index-header-bar-span">
              <a @click="toShopCar()"><img src="../../assets/shopcar.png" alt="" style="width: 20px;"> 购物车
              </a>
            </el-col>
            <el-col :span="3" :offset="1" class="index-header-bar-span">
              <a @click="toHaveMoney()">闲置卖钱</a>
            </el-col>
            <el-col :span="3" :offset="1" class="index-header-bar-span">
              <a href="#" data-toggle="dropdown">个人中心</a>
              <ul class="dropdown-menu my-dropdown-menu-1">
                <li>
                  <a class="my-dropdown-menu-li-1" @click="memberMessage">个人信息</a>
                </li>
                <li>
                  <a class="my-dropdown-menu-li-1" @click="checkLogin3">已购订单</a>
                </li>
                <li>
                  <a class="my-dropdown-menu-li-1" @click="checkLogin5">个人闲置</a>
                </li>
              </ul>
            </el-col>
          </el-row>
        </el-col>
      </el-row>
    </nav>

    <!--      弹出提示框-->
    <el-dialog
      title="提示"
      :visible.sync="centerDialogVisible"
      width="30%"
      center>
      <div style="text-align: center">
        <h4>{{ dialogValue }}</h4>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="centerDialogVisible = false" style="margin-top: 30px;">确 定</el-button>
        </span>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  //import { mapGetters } from 'vuex'
  export default {
    name: "index-header",
    data() {
      return {
        activeIndex: '1',
        centerDialogVisible: false,
        dialogValue: "",
        userName: "登录/注册"
      }
    },
    mounted() {

      this.userName = localStorage.getItem("userName");
      if (!this.userName)
        this.userName = "登录/注册"
      console.log("登錄" + this.userName)
    },
    methods: {
      // 退出
      exit() {
        this.userName = localStorage.setItem("userName", "登录/注册");
        this.userName = this.userName === undefined ? "登录/注册" : this.userName,
          console.log("退出" + this.userName)
      },
      //闲置卖钱
      toHaveMoney() {
        if (this.userName !== "登录/注册") {
          this.$router.push("/oldSail")
        } else {
          this.dialogValue = "请先登录账户";
          this.centerDialogVisible = true;
        }
      },
      // 购物车
      toShopCar() {
        if (this.userName !== "登录/注册") {
          this.$router.push("/shopCar")
        } else {
          this.dialogValue = "请先登录账户";
          this.centerDialogVisible = true;
        }
      },
      checkLogin1() {

        if (!window.sessionStorage.getItem("isLogin")) {
          this.$router.push("/login");
        } else {
          this.dialogValue = "请先登出账户";
          this.centerDialogVisible = true;
        }
      },
      checkLogin2() {
        if (!window.sessionStorage.getItem("isLogin")) {
          this.$router.push("/register");
        } else {
          this.dialogValue = "请先登出账户";
          this.centerDialogVisible = true;
        }
      },
      checkLogin3() {
        if (this.userName !== "登录/注册") {
          this.$router.push("/person#ordersDivId");
        } else {
          this.dialogValue = "请先登录账户";
          this.centerDialogVisible = true;
        }
      },
      memberMessage() {
        if (this.userName !== "登录/注册") {
          this.$router.push("/person#personInfoId");
        } else {
          this.dialogValue = "请先登录账户";
          this.centerDialogVisible = true;
        }
      },
      checkLogin5() {
        if (this.userName !== "登录/注册") {
          this.$router.push("/person#personOldId");
        } else {
          this.dialogValue = "请先登录账户";
          this.centerDialogVisible = true;
        }
      },
    }
  }
</script>

<style scoped>
  .index-header-bar {
    border-radius: 0;
    line-height: 36px;
    background-color: rgb(140, 34, 44);
    position: fixed;
    width: 100%;
    z-index: 999;
  }

  .navbar {
    height: 36px;
    min-height: 0;
  }

  .index-header-bar span {
    color: #e2c8ca;
    font-size: 12px;
    line-height: 34px;
  }

  .index-header-bar span:hover {
    color: white;
  }

  .index-header-bar-span {
    color: #e2c8ca;
    font-size: 12px;
    text-align: center;
    line-height: 34px;
    position: relative;
  }

  .index-header-bar-span:hover {
    background-color: rgb(123, 17, 27);
  }

  .index-header-bar a {
    color: #e2c8ca;
    font-size: 12px;
    line-height: 34px;
    text-decoration: none;
    border: 0;
  }

  .index-header-bar a:hover {
    color: white;
  }

  .my-dropdown-menu-1 {
    background-color: rgb(140, 34, 44);
    text-align: center;
    min-width: 80px;
  }

  .my-dropdown-menu-li-1:hover {
    background-color: rgb(123, 17, 27);
  }

  .my-el-menu-1 {
    border-bottom: 0;
  }

  .my-el-menu-1 > .el-menu-item.is-active {
    border-bottom: 2px solid rgb(181, 170, 154);
  }

  .my-el-menu-item-1 {
    margin-left: 5%;
    line-height: 50px;
    height: 50px;
  }
</style>

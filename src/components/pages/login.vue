<template>
  <div class="loginDivClass">
    <el-card shadow="always" style="top:50px;margin: 0 auto;width: 29%;min-height: 650px;position: relative;">
      <div style="text-align:center;margin-top: 30px;">
        <img src="../../assets/huigong1.png" alt="logoImg" style="max-width: 90px;">
        <h3 style="font-weight: 400;margin-top: 20px;">登录</h3>
      </div>
      <div style="text-align: center;margin-left: 20px;margin-top: 50px;">
        <el-input placeholder="请输入用户名" v-model="inputUsername" clearable maxlength="10"
                  show-word-limit>
          <template slot="prepend">用户名：</template>
        </el-input>
        <el-input placeholder="请输入密码" style="margin-top: 50px;" v-model="inputPassword" show-password maxlength="30"
                  show-word-limit>
          <template slot="prepend"><span style="padding-left: 7.2px;padding-right: 7.2px;">密码：</span></template>
        </el-input>
      </div>
      <div style="text-align: center;margin-left: 10px;margin-top: 110px;">
        <el-button type="primary" style="min-width: 80%;" @click="login()">登录</el-button>
      </div>
      <el-row type="flex" class="row-bg" justify="center" style="top: 60px;">
        <el-col :span="24">
          <h6 style="text-align: center;color: #757575;">吉首大学二手交易系统 版权所有</h6>
          <h6 style="text-align: center;color: #757575;">Copyright © 2019</h6>
        </el-col>
      </el-row>
    </el-card>

    <!--      弹出提示框-->
    <el-dialog
      title="提示"
      :visible.sync="centerDialogVisible"
      width="30%"
      center>
      <div style="text-align: center">
        <h4>{{ dialogValue }}</h4>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="clickButton" style="margin-top: 30px;">确 定</el-button>
          </span>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  //调用state的语法
  // import { mapMutations } from 'vuex'
  export default {
    name: "login",
    data() {
      return {
        "inputUsername": "",
        "inputPassword": "",
        "dialogValue": "登陆失败",
        "centerDialogVisible": false,
        "loginButton": false
      }
    },
    methods: {
      // ...mapMutations(["isLogin","setUserId"]),
      login() {
        //点击登录时显示确认弹框
        this.centerDialogVisible = true;
        let jsonObj = {};
        //获取填写的用户名和密码
        jsonObj.username = this.inputUsername;
        jsonObj.password = this.inputPassword;

        let jsonMsg = JSON.stringify(jsonObj);//填写的账号密码
        console.log(jsonMsg);
        let self = this;
        //transaction方法来打开数据库的事务
        //executeSql方法执行SQL语句
        //transaction有两个参数两个都是函数function
        // 第一个function写需要执行的executeSql方法，
        // 第二个function是执行失败的回调.
        this.$db.transaction(function (tx) {
          tx.executeSql('SELECT * FROM USER where account = "' + self.inputUsername + `"`, [], function (tx, results) {
            if (results.rows.length === 0) {
              alert("没有此用户，请先注册")
              return;
            }

            let pwd = results.rows.item(0).password;
            if (pwd !== self.inputPassword) {
              self.dialogValue = "密码错误";
            } else {
              self.dialogValue = "登录成功";
              console.log("登陸成功")
              localStorage.setItem("userName", self.inputUsername);

            }

          }, function (e) {
            console.log(e);
          });
        });

      },
      clickButton() {
        if (this.dialogValue === "登录成功") {
          this.$router.push({path: "/", query: {userName: this.inputUsername}});
        } else {
          this.centerDialogVisible = false;
        }
      }
    }
  }
</script>

<style scoped>
  .loginDivClass {
    background: url("https://i.loli.net/2019/12/15/A2XS7tNrHCM9f4R.png");
    height: 792px;
  }
</style>

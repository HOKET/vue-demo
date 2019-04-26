<template>
  <div id="fromPage">
    <h3>{{isReg?"请输入以下信息":"欢迎登录"}}</h3>
    <form v-if="!isReg">
      <div class="inp-line">
        <label>用户名：</label>
        <input type="text" v-model.trim="uid" placeholder="请输入您的用户名">
      </div>
      <div class="inp-line">
        <label>密码：</label>
        <input type="password" v-model.trim="password" placeholder="请输入您的密码">
      </div>
      <button class="btn-green" @click.prevent="login()">登录</button>
      <button @click.prevent="reg()">注册</button>
    </form>
    <form v-else>
      <div class="inp-line">
        <label>用户名：</label>
        <input type="text" v-model.trim="uid" placeholder="请输入您的用户名">
      </div>
      <div class="inp-line">
        <label>昵称：</label>
        <input type="text" v-model.trim="name" placeholder="请输入您的昵称">
      </div>
      <div class="inp-line">
        <label>密码：</label>
        <input type="password" v-model.trim="password" placeholder="请设置6-12位密码">
      </div>
      <div class="inp-line">
        <label>再次密码：</label>
        <input type="password" v-model.trim="repeat" placeholder="请再次输入您的密码">
      </div>
      <button class="btn-green" @click.prevent="addUser()">确定</button>
      <button @click.prevent="cancel()">返回</button>
    </form>
  </div>
</template>

<script>
import store from "@/store";
import { mapMutations, mapActions } from "vuex";
export default {
  name: "Login",
  data() {
    return {
      isReg: false,
      uid: "",
      password: "",
      repeat: "",
      name: ""
    };
  },
  store,
  methods: {
    //导入store.js中actions的两个方法
    // ...mapActions(["checkHasUser","checkLogin"]),
    reg() {
      this.isReg = true;
    },
    login() {
      var re = /^\w{6,12}$/;
      if (
        this.uid === "" ||
        this.password === "" ||
        !re.test(this.uid) ||
        !re.test(this.password)
      ) {
        alert("请输入正确的账号和密码");
        return;
      }
      var ls = localStorage;
      if (
        this.uid === ls.getItem("uid") &&
        this.password === ls.getItem("password")
      ) {
        this.$router.push("/home/list");
      } else {
        alert("账号或密码不正确");
      }
    },
    addUser() {
      var re = /^\w{6,12}$/;
      if (
        this.uid === "" ||
        this.password === "" ||
        !re.test(this.uid) ||
        !re.test(this.password)
      ) {
        alert("请输入正确的账号和密码");
        return;
      }
      if (this.password === this.repeat) {
        let uid = localStorage.getItem("uid")
        if(uid===this.uid){
          alert("该账号已注册");
          return;
        }
        if (this.name === "") {
          this.name = this.uid;
        }
        localStorage.setItem("uid", this.uid);
        localStorage.setItem("password", this.password);
        localStorage.setItem("name", this.name);
        this.isReg = true;
        alert("注册成功");
      } else {
        alert("两次密码输入不一致");
      }
    },
    cancel() {
      this.isReg = false;
    }
  }
};
</script>

<style scoped>
#fromPage {
  width: 90%;
  margin: 0 auto;
}
#fromPage > h3 {
  height: 50px;
  line-height: 50px;
}
#fromPage .inp-line {
  height: 40px;
  line-height: 40px;
  margin-bottom: 10px;
}
#fromPage label {
  display: block;
  text-align: right;
  width: 80px;
  float: left;
}
#fromPage input {
  width: 70%;
  float: left;
  height: 34px;
  padding: 3px;
  border-radius: 5px;
  border: 1px solid #bbbbbb;
}
#fromPage button {
  width: 45%;
  height: 40px;
  line-height: 40px;
  border-radius: 5px;
  border: 1px solid #42b983;
  font-size: 16px;
  color: #42b983;
  float: left;
  margin: 5px;
  background-color: #fff;
}
#fromPage .btn-green {
  background-color: #42b983;
  color: #fff;
}
</style>
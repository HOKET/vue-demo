<template>
  <div id="content">
    <h2>个人中心</h2>
    <h1>UID:{{uid}}</h1>
    <h1>昵称:{{name}}</h1>
    <button id="exit" @click="exit()">注销</button>
    <button @click="flag=!flag">重设密码</button>
    <div v-if="flag">
      <div class="inp-line">
        <label>旧密码:</label>
        <input type="password" v-model="oldPwd">
      </div>
      <div class="inp-line">
        <label>新密码:</label>
        <input type="password" v-model="newPwd">
      </div>
      <div class="inp-line">
        <label>再次密码:</label>
        <input type="password" v-model="repeat">
      </div>
      <button @click="resetPwd()">确定</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "User",
  data() {
    return {
      flag: false,
      uid: localStorage.getItem("uid"),
      name: localStorage.getItem("name"),
      oldPwd: "",
      newPwd: "",
      repeat: ""
    };
  },
  methods: {
    exit() {
      this.$router.push("/");
    },
    resetPwd() {
      var re = /^\w{6,12}$/;
      if (this.oldPwd === "" || this.newPwd === "" || !re.test(this.newPwd)) {
        alert("请输入正确的密码");
        return;
      }
      var ls = localStorage;
      if (this.newPwd === this.repeat) {
        if (ls.getItem("password") !== this.oldPwd) {
          alert("旧密码不正确");
          return;
        }
        ls.setItem("password", this.newPwd);
        this.oldPwd = this.newPwd = this.repeat = "";
        this.flag = false;
        alert("修改密码成功");
      } else {
        alert("两次密码输入不一致");
      }
    }
  }
};
</script>

<style scoped>
#content {
  width: 90%;
  margin: 10px auto;
}
#content > h3 {
  height: 50px;
  line-height: 50px;
}
#content > h1 {
  margin-top: 10px;
}
#content button{
    width:100%;
    height:50px;
    color:#fff;
    margin-top: 10px;
    background-color: #42b983;
    border-radius: 5px;
    border: 1px solid #bbbbbb;
}
#content .inp-line {
  height: 40px;
  line-height: 40px;
  margin-bottom: 10px;
}
#content label {
  display: block;
  text-align: right;
  width: 80px;
  float: left;
}
#content input {
  width: 70%;
  float: left;
  height: 34px;
  padding: 3px;
  border-radius: 5px;
  border: 1px solid #bbbbbb;
}
#exit{
    background-color: #ff4b4b !important;
}
</style>
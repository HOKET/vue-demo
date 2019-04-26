<template>
  <div id="content">
    <p>标题</p>
    <input type="text" v-model.trim="title">
    <p>内容</p>
    <textarea v-model="content"></textarea>
    <br>
    <button class="btn-green" @click="add()">提交</button>
    <button @click="back()">取消</button>
  </div>
</template>

<script>
import store from "@/store";
export default {
  name: "Add",
  store,
  data() {
    return {
      title: "",
      content: ""
    };
  },
  methods: {
    add() {
      if (this.title === "" || this.content === "") {
        alert("请填写信息");
        return;
      }
      store.commit("addItem", {
        title: this.title,
        content: this.content,
        name: localStorage.getItem('name'),
        date: new Date()
      });
      this.title = "";
      this.content = "";
      this.$router.push("/home/list");
    },
    back() {
      this.$router.push("/home/list");
    }
  }
};
</script>

<style scoped>
#content {
  width: 94%;
  margin: 10px auto;
  font-size: 16px;
}
#content input {
  width: 94%;
  margin: 10px auto;
  padding:5px;
  height: 30px;
  border-radius: 5px;
  border: 1px solid #bbbbbb;
}
textarea{
  width: 94%;
  margin: 10px auto;
  padding:5px;
  border-radius: 5px;
  border: 1px solid #bbbbbb;
  height:360px;
  text-align:justify
}

#content button {
  width: 46%;
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
#content .btn-green {
  background-color: #42b983;
  color: #fff;
}
</style>
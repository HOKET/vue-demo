<template>
  <div id="content">
    <h3>{{title}}</h3>
    <hr>
    <p>{{content}}</p>
    <button @click="back()">返回</button>
  </div>
</template>

<script>
import store from "@/store";
export default {
  name: "Info",
  store,
  data() {
    return {
      title: store.state.lists[this.$route.query.index].title,
      content: store.state.lists[this.$route.query.index].content
    };
  },
  methods: {
    back() {
        // console.log(store.state.lists[this.$route.query.index])
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
  width: 100%;
  height: 40px;
  line-height: 40px;
  border-radius: 5px;
  border: 1px solid #42b983;
  font-size: 16px;
  float: left;
  margin: 5px auto;
  margin-top: 20px;
  background-color: #42b983;
  color: #fff;
}
</style>
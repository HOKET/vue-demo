<template>
  <div>
    <router-view/>
    <ul class="footer">
      <li class="footer-tap">
        <router-link :to="{name:'list'}">文章列表</router-link>
      </li>
      <li class="footer-tap">
        <router-link :to="{name:'table'}">品牌管控</router-link>
      </li>
      <li class="footer-tap">
        <router-link :to="{name:'user'}">个人中心</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Home"
};
</script>
<style scoped>
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 60px;
  line-height: 60px;
  list-style: none;
  display: flex;
}
.footer-tap {
  flex: 1;
  text-align: center;
  border-top: 1px solid #42b983;
}
.footer-tap > a {
  color: #42b983;
  text-decoration: none;
  width: 100%;
  height: 100%;
  display: block;
}
.footer-tap > a.active {
  color: #fff;
  background-color: #42b983;
}
</style>
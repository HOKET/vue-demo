<template>
  <div>
    <button @click="send()">发布文章</button>
    <ul>
      <li v-for="(item,index) in getAllList" :key="index">
        <div>
          <div style="display:inline-block;width:100%">
            <span class="fl">{{item.title}}</span>
            <span class="fr">作者：{{item.name}}</span>
          </div>
          <hr>
          <p>{{item.content}}</p>
          <div>
            <span>{{item.date | dateFormat}}</span>
            <span class="fr"><router-link :to="`/home/info?index=${index}`">查看详情>></router-link></span>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import store from "@/store";
export default {
  name: "List",
  methods:{
    send(){
      this.$router.push('/add');
    },
    turn(index){
      this.$router.push("/home/info?index="+index);
    }
  },
  store,
  computed: {
    getAllList() {
      return store.state.lists;
    }
  },
  filters: {
    dateFormat: function (datastr, pattern = '') {
      let data = new Date(datastr)
      let year = data.getFullYear()
      let month = (data.getMonth() + 1).toString().padStart(2, '0')
      let day = data.getDate().toString().padStart(2, '0')
      let hour = data.getHours().toString().padStart(2, '0')
      let minute = data.getMinutes().toString().padStart(2, '0')
      let second = data.getSeconds().toString().padStart(2, '0')
      // return year + '-' + month + '-' + day
      if (pattern.toLowerCase() === 'yyyy-mm-dd') {
        return `${year}-${month}-${day}`
      } else {
        return `${year}-${month}-${day} ${hour}:${minute}:${second}`
      }
    }
  },
};
</script>

<style scoped>
button {
  display: block;
  width:94%;
  height: 34px;
  line-height: 34px;
  border-radius: 5px;
  border: 1px solid #42b983;
  font-size: 16px;
  color: #fff;
  margin: 10px auto;
  background-color: #42b983;
}
ul{
  width:94%;
  margin: 0 auto;
}
li{
  border: 1px solid #bbb;
  border-radius: 5px;
  padding:5px;
  box-shadow: 2px 2px 3px #bbb;
  margin-bottom: 10px;
}
li p{
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
li.active {
  color: #fff;
  background-color: #42b983;
}
.fl{
  display: block;
  float:left;
}
.fr{
  display: block;
  float:right;
}
</style>
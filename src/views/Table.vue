<template>
  <div>
    <div class="panel panel-primary">
      <div class="panel-heading">
        <h3 class="panel-title">添加品牌</h3>
      </div>
      <div class="panel-body">
        <label>
          Name:
          <input type="text" class="form-control" v-model="name">
        </label>
        <input type="button" value="添加" class="btn btn-primary" @click="add">
        <label>
          搜索名称关键字:
          <input type="text" class="form-control" v-model="keywords">
        </label>
      </div>
    </div>
    <!-- 表格 -->
    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th>ID</th>
          <th>名字</th>
          <th>时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in search(keywords)" :key="item.id">
          <td>{{item.id}}</td>
          <td>{{item.name}}</td>
          <td>{{item.ctime | dataFormat}}</td>
          <td>
            <a href @click.prevent="del(item.id)">删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import "../assets/jquery.min.js"
export default {
  name: "Table",
  data() {
    return {
      name: "",
      keywords: "",
      list: []
    };
  },
  methods: {
    getAllList() {
      var that = this;
      this.createRequest("GET", 'http://kerys.pythonanywhere.com/api/getprodlist/', null, function (data) {
          if(data.status === "success"){
            that.list = data.message;
          }
      })
    },
    add() {
      //添加方法
      if (!this.name) return;
      let index = this.list.findIndex(item => {
        if (item.name == this.name) {
          return true;
        }
      });
      if (index == -1) {
        var that = this;
        this.createRequest('POST', 'http://kerys.pythonanywhere.com/api/addprod/', { name: this.name }, function (data) {
          that.getAllList();
        })
      }
    },
    del(id) {
      let index = this.list.findIndex(item => {
        if (item.id == id) {
          return true;
        }
      });
      if (index != -1) {
        var that = this;
        this.createRequest("GET", "http://kerys.pythonanywhere.com/api/delprod/" + id, { id: id }, function(data) {
            that.getAllList();
          }
        );
      }
    },
    search(keywords) {
        let newList=[];
        if(this.list && this.list.length>0){
            newList = this.list.filter(item => {
                if (item.name.includes(keywords)) {
                    return true;
                }
            });
        }
      return newList;
    },
    createRequest(method, url, data, successFunc, failFunc) {
      if (!url) return;
      $.ajax({
        url: url, //处理页面的路径
        data: data, //要提交的数据是一个JSON
        type: method, //提交方式
        dataType: "json", //返回数据的类型
        // jsonp: 'callback',
        // jsonpCallback: 'callbackfunction',
        //TEXT字符串 JSON返回JSON XML返回XML
        contentType: "application/json;utf-8",
        success: successFunc, //回调函数 ,成功时返回的数据存在形参data里
        error: failFunc
      });
    }
  },
  filters: {
    dataFormat: function(datastr, pattern = "") {
      let data = new Date(datastr);
      let year = data.getFullYear();
      let month = (data.getMonth() + 1).toString().padStart(2, "0");
      let day = data
        .getDate()
        .toString()
        .padStart(2, "0");
      let hour = data
        .getHours()
        .toString()
        .padStart(2, "0");
      let minute = data
        .getMinutes()
        .toString()
        .padStart(2, "0");
      let second = data
        .getSeconds()
        .toString()
        .padStart(2, "0");
      // return year + '-' + month + '-' + day
      if (pattern.toLowerCase() === "yyyy-mm-dd") {
        return `${year}-${month}-${day}`;
      } else {
        return `${year}-${month}-${day} ${hour}:${minute}:${second}`;
      }
    }
  },
  created() {
    this.getAllList(); //AJAX请求
  }
};
</script>

<style scoped>
@import url(../assets/boot-strap.css);
</style>
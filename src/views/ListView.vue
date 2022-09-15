<template>
  <div>
    <table border="1px" align="center">
      <tr>
        <th>编号</th>
        <th>名称</th>
        <th>性别</th>
        <th>年龄</th>
        <th>爱好</th>
        <th>部门</th>
        <th>操作</th>
      </tr>

      <tr v-for="b in bean.list" :key="b.id">
        <td>{{ b.id }}</td>
        <td>{{ b.name }}</td>
        <td>{{ b.sex }}</td>
        <td>{{ b.age }}</td>
        <td>{{ b.hobby }}</td>
        <td>{{ b.department.name }}</td>
        <td>
          <button type="button" @click="fnPreUpdate(b.id)">修改</button>
          <button type="button" @click="fnDelete(b.id)">删除</button>
        </td>
      </tr>
    </table>
    <div>
      当前是第{{ bean.pageNum }}页，共{{ bean.pages }}页，共{{ bean.total }}条
      <span v-for="n in bean.navigatepageNums" :key="n">
          &nbsp;&nbsp;<a href="javascript:void(0)" @click="fnGo(n)">{{ n }}</a>&nbsp;&nbsp;
      </span>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ListView",
  data() {
    return {
      bean: {
        list: []
      }
    }
  },
  mounted() {
    this.fnGo();
  },
  methods: {
    fnGo: function (p = 1) {
      let that = this;
      axios.get("http://localhost:8081/emp/list?page=" + p)
          .then(res => {
            if (res.data.code === "10000") {
              that.bean = res.data.data;
              console.log(that.bean)
            } else {
              //alert(res.data.desc)
              alert("未知异常")
            }
          });
    }
  },
  fnDelete: function (id) {
    let that = this;
    if (confirm("确定要删除吗？")) {
      axios.get("http://localhost:8081/emp/delete/" + id)
          .then(res => {
            if (res.data.code === "10000") {
              alert("删除成功");
              that.fnGo();
            } else {
              alert(res.data.desc);
            }
          })
    }
  },
  fnPreUpdate: function (id) {
    sessionStorage.setItem("empId", id);
    // 路由跳转
    this.$router.push("/update");
  }
}

</script>

<style scoped>

</style>
<!-- 路由-->

<template>
  <div>
    <el-container style="height: 100%; border: 1px solid #eee">
      <el-header style="font-size:40px;background-color: rgb(238,241,246)">University Of Concordia Admin</el-header>
      <el-container>
        <el-aside width="210px" style="border:  1px solid #eee">
          <el-menu :default-openeds="['1', '3']">
            <el-submenu index="1">
              <template slot="title"><i class="el-icon-message"></i>导航一</template>
              <el-menu-item index="1-1">
                <router-link to="/dept">Dept Admin</router-link>
              </el-menu-item>
              <el-menu-item index="1-2">
                <router-link to="/emp">Emp Admin</router-link>
              </el-menu-item>
            </el-submenu>
          </el-menu>
        </el-aside>
        <el-container>
          <el-main>
            <!--            @click="showMessageBox()-->
            <el-button class="Left_Button" @click="Add()">Add a Dept</el-button>
            <el-table :data="tableData" border height="500px">
              <el-table-column prop="id" label="id">
              </el-table-column>
              <el-table-column prop="name" label="name">
              </el-table-column>
              <el-table-column prop="createTime" label="createTime">
              </el-table-column>
              <el-table-column prop="updateTime" label="updateTime">
              </el-table-column>
              <el-table-column label="Adjust">
                <template slot-scope="scope">
                  <el-button type="primary" size="medium" @click=edit(scope.row.id)>edit</el-button>
                  <el-button type="danger" size="medium" @click="deleteRow(scope.row.id)">delete</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-main>
        </el-container>
      </el-container>
    </el-container>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      tableData: []
    }
  },
  methods: {
    Add() {
      this.$prompt('Please enter the name of the Department', '提示', {
        confirmButtonText: 'confirm',
        cancelButtonText: 'cancel',
        inputPattern: /^[a-zA-Z]+(?:\s[a-zA-Z]+)*$/,
        inputErrorMessage: 'the form is not correct'
      }).then(({value}) => {
        const dept = {name: value};
        axios.post('http://localhost:8080/depts', dept).then(() => {
          this.DataRefresh(); // Call DataRefresh after successful post
        })
      })
    },
    edit(id1) {
      let Current_Dept_Name;
      console.log(id1);
      axios
          .get(`http://localhost:8080/depts/${id1}`)
          .then((response) => {
            Current_Dept_Name = response.data.data[0].name;
            console.log(Current_Dept_Name, 'h');
            return this.$prompt('The name of the department:', 'Update', {
              confirmButtonText: 'confirm',
              cancelButtonText: 'cancel',
              inputValue: Current_Dept_Name,
              inputPattern: /^[a-zA-Z]+(?:\s[a-zA-Z]+)*$/,
              inputErrorMessage: 'the form is not correct'
            });
          })
          .then(({value}) => {
            const dept = { id :id1, name : value};
            console.log(dept);
            return axios.put('http://localhost:8080/depts', dept);
          })
          .then(() => {
            this.DataRefresh(); // Call DataRefresh after successful post
            this.$notify({
              title: 'Edit',
              message: 'You have successfully edited it!',
              type: 'success'
            });
          })
    },
    deleteRow(id) {
      axios.delete(`http://localhost:8080/depts/${id}`)
          .then(() => {
            this.tableData = this.tableData.filter((row) => row.id !== id);
            this.$notify({
              title: 'Delete',
              message: 'You have successfully delete it !',
              type: 'success'
            });
          })
          .catch((error) => {
            console.error(error);
          });
    },
    DataRefresh() {
      console.log("Silent Refreshed")
      axios.get('http://localhost:8080/depts')
          .then((response) => {
            this.tableData = response.data.data;
          })
          .catch((error) => {
            console.error(error);
          });
    }
  },
  mounted() {
    axios.get('http://localhost:8080/depts')
        .then((response) => {
          this.tableData = response.data.data;
        })
        .catch((error) => {
          console.error(error);
        });
  }
}
</script>
<style>
.Left_Button {
  Float: left;
  margin-bottom: 10px;
  background-color: #409EFF;
  color: #FFF;
}
</style>

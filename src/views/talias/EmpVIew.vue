<template>
  <div>
    <el-container style="height: 100%; ">
      <el-header style="font-size:40px;background-color: rgb(238,241,246)">University Of Concordia Admin</el-header>
      <el-container>
        <el-aside width="210px" style="border:1px solid #eee">
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
            <el-form :inline="true" :model="searchForm" class="demo-form-inline">
              <el-form-item label="name">
                <el-input v-model="searchForm.name" placeholder="name"></el-input>
              </el-form-item>
              <el-form-item label="gender">
                <el-select v-model="searchForm.gender" placeholder="gender">
                  <el-option label="male" value="1"></el-option>
                  <el-option label="female" value="2"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="Start">
                <el-date-picker
                    v-model=searchForm.entrydate
                    type="daterange"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期">
                </el-date-picker>
              </el-form-item>

              <el-form-item>
                <el-button type="primary" @click="onSubmit">Search</el-button>
              </el-form-item>

            </el-form>
            <div class="table_data">
              <el-table :data="tableData" border height="600px">
                <el-table-column prop="id" label="id" width="50px">
                </el-table-column>
                <el-table-column prop="username" label="username" width="100px">
                </el-table-column>
                <el-table-column prop="password" label="password" width="100px">
                </el-table-column>
                <el-table-column prop="name" label="name" width="130px">
                </el-table-column>
                <el-table-column prop="gender" label="gender" width="80px">
                  <!--                <template slot-scope="scope">-->
                  <!--                  {{ scope.row.gender === 1 ? 'M' : 'F' }}-->
                  <!--                </template>-->
                </el-table-column>
                <el-table-column prop="image" label="image" width="130px">
                  <!--                <template slot-scope="scope">-->
                  <!--                  <img :src="scope.row.image" height="50px">-->
                  <!--                </template>-->
                </el-table-column>
                <el-table-column prop="job" width="50px" label="job">
                </el-table-column>
                <el-table-column prop="entrydate" width="95px" label="entrydate">
                </el-table-column>
                <el-table-column prop="deptId" width="50px" label="deptId">
                </el-table-column>
                <el-table-column prop="createTime" width="100px" label="createTime">
                </el-table-column>
                <el-table-column prop="updateTime" width="100px" label="updateTime">
                </el-table-column>
                <el-table-column label="Adjust" width="150px">
                  <el-button type="primary" size="mini">edit</el-button>
                  <el-button type="danger" size="mini">delete</el-button>
                </el-table-column>
              </el-table>
            </div>
            <br>
            <!-- page select-->
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page=1
                :page-sizes="[5,10,15,20]"
                :page-size="5"
                layout="total, sizes, prev, pager, next, jumper"
                :total="totalRows">
            </el-pagination>
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
      totalRows: 0,
      currentPage: 1, // Current page number
      pageSize: 5, // Number of items per page
      tableData: [],
      searchForm: {
        name: '',
        gender: '',
        entrydate: []
      }

    }
  },
  methods: {

    handleSizeChange(val) {
      console.log(val);
      this.pageSize = val
      this.paging();
    },
    handleCurrentChange(val) {
      console.log(val);
      this.currentPage = val
      this.paging();
    },
    onSubmit: function () {
      alert('about to search')
    },
    paging() {
      console.log(this.currentPage, 'and size:', this.pageSize)
      axios.get(`http://localhost:8080/emps?page=${this.currentPage}&pageSize=${this.pageSize}`).then((result) => {
        this.tableData = result.data.data.rows
      })
    },
  },
  mounted() {
    axios.get('http://localhost:8080/emps?page=1&pageSize=5').then((result) => {
      this.tableData = result.data.data.rows
      this.totalRows = result.data.data.total
    })
  }

}
</script>
<style>
.table_data {
  margin: auto;
  width: fit-content;
}
</style>
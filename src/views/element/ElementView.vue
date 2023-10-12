<template>
  <div>
    <el-row>
      <el-button>默认按钮</el-button>
      <el-button type="primary">主要按钮</el-button>
      <el-button type="success">成功按钮</el-button>
      <el-button type="info">信息按钮</el-button>
      <el-button type="warning">警告按钮</el-button>
      <el-button type="danger">危险按钮</el-button>
    </el-row>

    <el-table
        :data="tableData"
        border
        style="width: 100%">
      <el-table-column
          prop="date"
          label="日期"
          width="180">
      </el-table-column>
      <el-table-column
          prop="name"
          label="姓名"
          width="180">
      </el-table-column>
      <el-table-column
          prop="address"
          label="地址">
      </el-table-column>
    </el-table>

    <el-pagination
        background
        layout=" sizes, prev, pager, next, jumper,total, slot"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :total="1000">
    </el-pagination>

    <br>
    <br>
    <!-- Table -->
    <!-- Table -->
    <el-button type="text" @click="dialogFormVisible = true">Dialog for Table</el-button>

    <el-dialog title="收货地址" :visible.sync="dialogFormVisible">
      <el-table :data="gridData">
        <el-table-column property="date" label="日期" width="150"></el-table-column>
        <el-table-column property="name" label="姓名" width="200"></el-table-column>
        <el-table-column property="address" label="地址" width="200"></el-table-column>
      </el-table>
    </el-dialog>
    <br>
<!--    &lt;!&ndash; Form &ndash;&gt;-->
<!--    <el-button type="text" @click="dialogFormVisible = true">Dialog for Form</el-button>-->

<!--    <el-dialog title="收货地址" :visible.sync="dialogFormVisible">-->
<!--      <el-form :model="form">-->
<!--        <el-form-item label="活动名称" :label-width="formLabelWidth">-->
<!--          <el-input v-model="form.name" autocomplete="off"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="活动区域" :label-width="formLabelWidth">-->
<!--          <el-select v-model="form.region" placeholder="请选择活动区域">-->
<!--            <el-option label="区域一" value="shanghai"></el-option>-->
<!--            <el-option label="区域二" value="beijing"></el-option>-->
<!--          </el-select>-->
<!--        </el-form-item>-->
<!--      </el-form>-->
<!--      <div slot="footer" class="dialog-footer">-->
<!--        <el-button @click="dialogFormVisible = false">取 消</el-button>-->
<!--        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>-->
<!--      </div>-->
<!--    </el-dialog>-->
    <!--dialog 对话框 表单form-->
    <br>    <br>

    <el-button type="text" @click="dialogTableVisible = true">open embedded form-Dialog</el-button>
    <el-dialog title="Calender" :visible.sync="dialogTableVisible">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="Activity">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="Location">
          <el-select v-model="form.region" placeholder="Please choose Airport">
            <el-option label="YUL" value="Montreal"></el-option>
            <el-option label="YYZ" value="Toronto"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Time">
          <el-col :span="11">
            <el-date-picker type="date" placeholder="choose date" v-model="form.date1" style="width: 100%;"></el-date-picker>
          </el-col>
          <el-col class="line" :span="2">-</el-col>
          <el-col :span="11">
            <el-time-picker placeholder="choose time" v-model="form.date2" style="width: 100%;"></el-time-picker>
          </el-col>
        </el-form-item>
        <el-form-item label="Lounge">
          <el-switch v-model="form.delivery"></el-switch>
        </el-form-item>
        <el-form-item label="Purpose">
          <el-checkbox-group v-model="form.type">
            <el-checkbox label="business" name="type"></el-checkbox>
            <el-checkbox label="travel" name="type"></el-checkbox>
            <el-checkbox label="transfer" name="type"></el-checkbox>
           </el-checkbox-group>
        </el-form-item>
        <el-form-item label="Payment">
          <el-radio-group v-model="form.resource">
            <el-radio label="Amex"></el-radio>
            <el-radio label="Mastercard"></el-radio>
            <el-radio label="Visa"></el-radio>

          </el-radio-group>
        </el-form-item>
        <el-form-item label="Notice">
          <el-input type="textarea" v-model="form.desc"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">Create notice</el-button>
          <el-button>Cancel</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>


<script>
export default {
  data() {
    return {
      gridData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }],
      dialogTableVisible: false,
      dialogFormVisible: false,
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      formLabelWidth: '120px',

      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }]
    }
  },
  methods: {
    handleSizeChange: function (val) {
      alert('each year record changed ' + val)
    },
    handleCurrentChange: function (val) {
      alert('Page changed ' + val)
    },
    onSubmit:function() {
      alert(JSON.stringify(this.form));
      console.log('submit!');
    }
  }
}

</script>

<style>


</style>


{
}
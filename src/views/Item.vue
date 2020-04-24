<template>
  <div id="item">
    <el-button @click="showDialog(null)" type="primary" slot="reference" icon="el-icon-plus" circle></el-button>

    <div id="table">
      <el-table :data="tableData" style="width: 100%" v-loading="loading">
        <el-table-column prop="itemName" label="监控名" min-width="260"></el-table-column>
        <el-table-column prop="mappingName" label="映射名" min-width="160"></el-table-column>
        <el-table-column prop="weight" label="权重值" min-width="160"></el-table-column>
        <el-table-column label="修改" width="70">
          <template slot-scope="scope">
            <el-button
              @click="showDialog(scope.row)"
              type="warning"
              slot="reference"
              icon="el-icon-s-tools"
              circle
            ></el-button>
          </template>
        </el-table-column>

        <el-table-column label="删除" width="70">
          <template slot-scope="scope">
            <el-popconfirm
              confirmButtonText="确定"
              cancelButtonText="我再想想"
              icon="el-icon-info"
              iconColor="red"
              title="确定删除吗？"
              @onConfirm="del(scope.row)"
            >
              <el-button type="danger" slot="reference" icon="el-icon-delete" circle></el-button>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <br />
    <div id="pagination">
      <el-pagination
        :page-size="pageSize"
        :pager-count="5"
        layout="prev, pager, next"
        :total="totalElements"
        @current-change="handleCurrentChange"
      ></el-pagination>
    </div>

    <el-dialog title="监控项管理" :visible.sync="dialogFormVisible">
      <el-form>
        <el-form-item label="监控名" label-width="120">
          <el-input v-model="itemName" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="映射名" label-width="120">
          <el-input v-model="mappingName" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="权重" label-width="120">
          <el-select v-model="weight" placeholder="权重">
            <el-option label="1" value="1"></el-option>
            <el-option label="2" value="2"></el-option>
            <el-option label="3" value="3"></el-option>
            <el-option label="4" value="4"></el-option>
            <el-option label="5" value="5"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="flag===0?add():update()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      pageNum: 0,
      pageSize: 6,
      totalElements: 4,
      loading: false,
      dialogFormVisible: false,
      itemId: 0,
      itemName: "",
      mappingName: "",
      weight: 1,
      flag: -1
    };
  },
  methods: {
    listExcel() {
      this.loading = true;
      this.$axios({
        method: "GET",
        url:
          "/data/listItem?pageNum=" +
          this.pageNum +
          "&pageSize=" +
          this.pageSize
      }).then(result => {
        if (result.status === 200) {
          var page = result.data.data;
          this.tableData = page.content;
          this.totalElements = page.totalElements;
          this.pageNum = page.number;
          this.size = page.size;
          this.loading = false;
        } else {
          this.$message.error("出错了");
        }
      });
    },
    add() {
      this.$axios({
        method: "POST",
        url: "/data/addItem",
        data: {
          itemName: this.itemName,
          mappingName: this.mappingName,
          weight: this.weight
        }
      }).then(result => {
        if (result.status === 200) {
          this.$message.success("新增成功");
        } else {
          this.$message.error("出错了");
        }
      });
      this.dialogFormVisible = false;
      this.listExcel();
      //将flag重置
      this.flag = -1;
    },
    update() {
      this.$axios({
        method: "PUT",
        url: "/data/updateItem",
        data: {
          id: this.itemId,
          itemName: this.itemName,
          mappingName: this.mappingName,
          weight: this.weight
        }
      }).then(result => {
        if (result.status === 200) {
          this.$message.success("修改成功");
            this.listExcel();
        } else {
          this.$message.error("出错了");
        }
      
      });
      this.dialogFormVisible = false;
   
      //将flag重置
      this.flag = -1;
    },
    del(row) {
      this.$axios({
        method: "DELETE",
        url: "/data/delete/" + row.id,
        data: {}
      }).then(result => {
        if (result.status === 200) {
          this.$message.success("删除成功");
        } else {
          this.$message.error("出错了");
        }
      });
      this.listExcel();
    },
    handleCurrentChange(val) {
      this.pageNum = val - 1;
      this.listExcel();
    },
    showDialog(row) {
      if (row !== null) {
        //如果是更新调用，设置flag=1
        this.flag = 1;
        this.itemId = row.id;
        this.itemName = row.itemName;
        this.mappingName = row.mappingName;
        this.weight = row.weight;
      } else {
        //如果是新增调用，设置flag=0
        this.flag = 0;
        this.itemId = "";
        this.itemName = "";
        this.mappingName = "";
        this.weight = 0;
      }
      this.dialogFormVisible = true;
    }
  },
  created() {
    this.listExcel();
  }
};
</script>

<style scoped>
#table {
  height: 1024;
}
</style>
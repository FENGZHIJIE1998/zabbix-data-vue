<template>
  <div id="history">
    <div id="table">
      <el-table :data="tableData" style="width: 100%"   v-loading="loading"> 
      
        <el-table-column prop="title" label="标题" min-width="260"></el-table-column>
     
        <el-table-column prop="createTime" label="生成时间" min-width="160"></el-table-column>
        <el-table-column prop="url" label="下载">
          <template slot-scope="scope">
            <el-button @click="download(scope.row)" type="primary" slot="reference" icon="el-icon-download" circle></el-button>
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
      loading: false
    };
  },
  methods: {
    listExcel() {
       this.loading=true;
      this.$axios({
        method: "GET",
        url:
          "/data/listExcel?pageNum=" +
          this.pageNum +
          "&pageSize=" +
          this.pageSize
      }).then(result => {
        //console.log(result);
        if (result.status === 200) {
          var page = result.data.data;
          //console.log(page);
          this.tableData = page.content;
          this.totalElements = page.totalElements;
          this.pageNum = page.number;
          this.size = page.size;
          this.loading=false;
        }else{
             this.$message.error('出错了');
        }
      });
           
    },
    handleCurrentChange(val) {
      //console.log(val-1);
      this.pageNum = val - 1;
      this.listExcel();
    },
    download(data) {
         var a = document.createElement("a");
            a.download = data.title;
            a.href = "/data/"+data.url
            a.click();
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
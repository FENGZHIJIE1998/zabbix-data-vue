<template>
  <div class="excel">
    <div>请选择要统计的时间范围</div>
    <div>时间范围：指定日期的00:00:00至指定日期的23:59:59</div>
    <br />
    <div class="block">
      <el-date-picker
        v-model="date"
        type="daterange"
        align="right"
        unlink-panels
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        value-format="timestamp"
        :picker-options="pickerOptions"
      ></el-date-picker>
    </div>
    <br />
    <el-button @click="getExcel" type="primary">生成Excel</el-button>
  </div>
</template>

<script>
export default {
  name: "Excel",
  data() {
    return {
      pickerOptions: {
        shortcuts: [
          {
            text: "最近一周",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近一个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近三个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit("pick", [start, end]);
            }
          }
        ]
      },
      date: ""
    };
  },
  methods: {
    //生成excel
    getExcel() {
      if (this.date === "" || this.date[0] === "" || this.date[1] == "") {
        this.$message.warning("请选择时间");
        return;
      }
      const loading = this.$loading({
        lock: true,
        text: "生成中..",
        spinner: "el-icon-loading",
        background: "rgba(0, 0, 0, 0.7)"
      });

      const time_from = Math.round(this.date[0] / 1000);
      const time_till = Math.round(this.date[1] / 1000 + 86399);
      this.$axios({
        method: "GET",
        url: "/data/getExcel?time_from=" + time_from + "&time_till=" + time_till
      }).then(result => {
        console.log(result);

        if (result.data.status === 200) {
          var a = document.createElement("a");
          a.download = result.data.data.title;
          a.href = "/data/" + result.data.data.url;
          a.click();
          this.$message.success("生成成功");
          this.date = "";
        } else {
          this.$message.danger("生成失败");
        }

        loading.close();
      });
    }
  }
};
</script>

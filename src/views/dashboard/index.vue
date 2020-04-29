<template>
  <div class="dashboard-container">
    <!-- 导入按钮 -->
    <div class="button_group">
      <a
        href="javascript:;"
        class="button_s my_file el-button button_s el-button--primary el-button--small"
      >
        <input type="file" class="my_input" @change="importExcel" id="upload" />导入
      </a>
    </div>
    <!-- 导入按钮 -->
    <!-- 表格组件 -->
    <div class="myTable">
      <el-table
        max-height="600"
        :data="dataArr"
        v-loading="tableLoading"
        :span-method="objectSpanMethod"
        border
        style="width: 100%"
      >
        <el-table-column
          :prop="item.prop"
          :label="item.label"
          :width="item.width"
          v-for="(item, i) in tableColumn"
          :key="i"
        ></el-table-column>
      </el-table>
    </div>
    <!-- 表格组件 -->
  </div>
</template>

<script>
import xlsx from "xlsx";
export default {
  name: "Dashboard",
  data() {
    return {
      dataArr: [], // 表格内容数据数组
      // countArr: {}, // 分析表格数据以及表头，得到一个对照数组，用来进行自定义合并，本文暂时只写基础，不介绍自动合并单元格了哟~~我的其他文章有写自定义合并实现方法~
      tableColumn: [], // 表格表头配置数组
      tableLoading: false // 表格是否loading
    };
  },
  methods: {
    /**
     * 设置表格渲染合并单元格，本文暂时只写基础，不介绍自动合并单元格了哟~~我的其他文章有写自定义合并实现方法~
     */
    objectSpanMethod({ row, column, rowIndex, columnIndex }) {
      // if (columnIndex < 5) { // 举例只自动合并前4列
      //   if (this.countArr[column.property]) {
      //     let colNum = this.countArr[column.property][rowIndex];
      //     return {
      //       rowspan: colNum,
      //       colspan: 1
      //     };
      //   }
      // }
    },
    /**
     * 获取表格表头
     */
    getHeader(sheet) {
      const XLSX = xlsx;
      const headers = [];
      const range = XLSX.utils.decode_range(sheet["!ref"]); // worksheet['!ref'] 是工作表的有效范围
      let C;
      /* 获取单元格值 start in the first row */
      const R = range.s.r; // 行 // C 列
      let i = 0;
      for (C = range.s.c; C <= range.e.c; ++C) {
        var cell =
          sheet[
            XLSX.utils.encode_cell({ c: C, r: R })
          ]; /* 根据地址得到单元格的值find the cell in the first row */
        var hdr = "UNKNOWN" + C; // 如果有空表头，会替换为您想要的默认值replace with your desired default
        // XLSX.utils.format_cell 生成单元格文本值
        if (cell && cell.t) hdr = XLSX.utils.format_cell(cell);
        if(hdr.indexOf('UNKNOWN') > -1){
          if(!i) {
            hdr = '__EMPTY';
          }else {
            
            hdr = '__EMPTY_' + i;
          }
          i++;
        }
        headers.push(hdr);
      }
      return headers;
    },
    setTable(headers, excellist) {
      const tableTitleData = []; // 存储表格表头数据
      const tableMapTitle = {}; // 设置表格内容中英文对照用
      headers.forEach((_, i) => {
        tableMapTitle[_] = "prop" + i;
        tableTitleData.push({
          prop: "prop" + i,
          label: _,
          width: 100
        });
      });
      console.log("tableTitleData", tableTitleData);
      // 映射表格内容属性名为英文
      const newTableData = [];
      excellist.forEach(_ => {
        const newObj = {};
        Object.keys(_).forEach(key => {
          newObj[tableMapTitle[key]] = _[key];
        });
        newTableData.push(newObj);
      });
      console.log('newTableData',newTableData);
      this.tableColumn = tableTitleData;
      this.dataArr = newTableData;
    },
    /**
     * 导入表格
     */
    importExcel(e) {
      const files = e.target.files;
      console.log(files);
      if (!files.length) {
        return;
      } else if (!/\.(xls|xlsx)$/.test(files[0].name.toLowerCase())) {
        return alert("上传格式不正确，请上传xls或者xlsx格式");
      }
      const fileReader = new FileReader();
      fileReader.onload = ev => {
        try {
          const data = ev.target.result;
          const XLSX = xlsx;
          const workbook = XLSX.read(data, {
            type: "binary"
          });
          const wsname = workbook.SheetNames[0]; //取第一张表，wb.SheetNames[0]是获取Sheets中第一个Sheet的名字
          const ws = XLSX.utils.sheet_to_json(workbook.Sheets[wsname]); //生成json表格内容，wb.Sheets[Sheet名]获取第一个Sheet的数据
          const excellist = []; //清空接收数据
          //编辑数据
          for (var i = 0; i < ws.length; i++) {
            excellist.push(ws[i]);
          }
          console.log("读取结果", excellist); // 此时得到的是一个内容是对象的数组需要处理
          // 获取表头1-1
          const a = workbook.Sheets[workbook.SheetNames[0]];
          const headers = this.getHeader(a);
          console.log("headers", headers);
          // 获取表头1-2
          // 渲染表格1-1
          this.setTable(headers, excellist);
          // 渲染表格1-2
        } catch (e) {
          return alert("读取失败!");
        }
      };
      fileReader.readAsBinaryString(files[0]);
      var input = document.getElementById("upload");
      input.value = "";
    }
  }
};
</script>

<style lang="scss" scoped>
// 按钮样式
.button_group {
  .button_s {
    width: 78px;
    margin: 5px 10px 5px 5px;
  }
  .button_m {
    width: 100px;
    margin: 5px 10px 5px 5px;
  }
  .my_file {
    position: relative;
    .my_input {
      position: absolute;
      opacity: 0;
      width: 78px;
      height: 30px;
      top: 0;
      left: 0;
    }
  }
}
// 按钮样式
</style>

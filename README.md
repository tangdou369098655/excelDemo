# @[A Practical Front end reading excel table Getting Started Guide With Vue and XLSX](https://github.com/tangdou369098655/excelDemo/blob/master/README-zh.md)

English | [简体中文](./README-zh.md)


> In this tutorial, we are going to implement Front end reading excel tables and get the content  of the file without back-end interface.


* Use it to learn important XLSX features.
* My other articles address expansion requirements：
* **Automatic cell merging** after reading and parsing excel tables
* **Set cell color automatically** after read and analyze the excel tables , then compare and analyze according to the data.
* Perform data analysis after **Reading the analysis excel tables（dialysis）** Generate the **echarts** diagram that can meet the user-defined requirements.
* Download excel tables

## Introduction

There are many **Background Management Systems** in my company. It's very common to upload, parse and download excel tables. I've written similar posts before, but I only put the code and never write comments and steps. Haha, I don't say much. This article is a complete record: 

## Prerequisites

> ####  I usually use the following techniques：
> ####  **Ant Design + Angular**
> ####  **Element UI + Vue**
> ####  **Ant Design + Vue**
> ####  For convenience ,today we use **Element UI + Vue**
> ####  Start directly based on **vue-element-admin**

## Step 1：Preparation

### 1. [Click to download vue-element-admin](https://github.com/PanJiaChen/vue-admin-template)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430005909216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 2. Download and unzip

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430010324823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 3. Install dependency and run

Your project should resemble the following:


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011233719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011409967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011530155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 4. Run successfully

You will see a  Login Form System web interface.

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011749527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430012004341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

## Step2：Import and parse tables

 ### 1. Enter the following path；

> src\views\dashboard\index.vue

 ### 2. Delete useless code, prepare to start；

 ```javascript
<template>
  <div class="dashboard-container">
  </div>
</template>

<script>
export default {
  name: 'Dashboard'
}
</script>

<style lang="scss" scoped>

</style>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430012847944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 3. Add * * import * * button


```javascript
<template>
  <div class="dashboard-container">
    <!-- Import button -->
    <div class="button_group">
      <a
        href="javascript:;"
        class="button_s my_file el-button button_s el-button--primary el-button--small"
      >
        <input type="file" class="my_input" @change="importExcel" id="upload" />Import
      </a>
    </div>
    <!-- Import button -->
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  methods: {
    /**
     * Import table
     */
     importExcel(e) {
    }
  }
}
</script>

<style lang="scss" scoped>
// Button style
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
// Button style
</style>
```

 ### 4. Save and refresh；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430014652773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 5.  **Download xlsx** And Import xlsx；

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430014838456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430015001318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 6. Write * * Import Table * * function code, Save and Refresh；

```javascript
<script>
import xlsx from "xlsx";
export default {
  name: 'Dashboard',
  methods: {
    /**
     * Import table
     */
     importExcel(e) {
      const files = e.target.files;
      console.log(files);
      if (!files.length) {
        return ;
      } else if (!/\.(xls|xlsx)$/.test(files[0].name.toLowerCase())) {
        return alert("The upload format is incorrect. Please upload xls or xlsx format");
      }
      const fileReader = new FileReader();
      fileReader.onload = ev => {
        try {
          const data = ev.target.result;
          const XLSX = xlsx;
          const workbook = XLSX.read(data, {
            type: "binary"
          });
          const wsname = workbook.SheetNames[0]; // Take the first sheet，wb.SheetNames[0] :Take the name of the first sheet in the sheets
          const ws = XLSX.utils.sheet_to_json(workbook.Sheets[wsname]); // Generate JSON table content，wb.Sheets[Sheet名]    Get the data of the first sheet
          const excellist = [];  // Clear received data
          // Edit data
          for (var i = 0; i < ws.length; i++) {
            excellist.push(ws[i]);
          }
          console.log("Read results", excellist); // At this point, you get an array containing objects that need to be processed
        } catch (e) {
          return alert("Read failure!");;
        }
      };
      fileReader.readAsBinaryString(files[0]);
      var input = document.getElementById("upload");
      input.value = "";
    }
  }
}
</script>
```

 ### 7.  Write the following table to * * test the function** ；

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020043002180586.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430021859321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430021954425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 8. Sometimes, the table title is Chinese. If we want to get the English attribute name in Object After the table is parsed successfully ,  **Add The Following Code * *  and test again;


```javascript
<script>
import xlsx from "xlsx";
export default {
  name: 'Dashboard',
  methods: {
    getHeader(sheet) {
      const XLSX = xlsx;
      const headers = [];
      const range = XLSX.utils.decode_range(sheet["!ref"]); // worksheet['!ref'] Is the valid range of the worksheet
      let C;
      /* Get cell value start in the first row */
      const R = range.s.r; //Line / / column C
      let i = 0;
      for (C = range.s.c; C <= range.e.c; ++C) {
        var cell =
          sheet[
            XLSX.utils.encode_cell({ c: C, r: R })
          ]; /* Get the cell value based on the address  find the cell in the first row */
        var hdr = "UNKNOWN" + C; // replace with your desired default
        // XLSX.utils.format_cell Generate cell text value
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
    /**
     * Import table
     */
     importExcel(e) {
      const files = e.target.files;
      console.log(files);
      if (!files.length) {
        return ;
      } else if (!/\.(xls|xlsx)$/.test(files[0].name.toLowerCase())) {
        return alert("The upload format is incorrect. Please upload xls or xlsx format");
      }
      const fileReader = new FileReader();
      fileReader.onload = ev => {
        try {
          const data = ev.target.result;
          const XLSX = xlsx;
          const workbook = XLSX.read(data, {
            type: "binary"
          });
          const wsname = workbook.SheetNames[0]; // Take the first sheet，wb.SheetNames[0] :Take the name of the first sheet in the sheets
          const ws = XLSX.utils.sheet_to_json(workbook.Sheets[wsname]); // Generate JSON table content，wb.Sheets[Sheet名]    Get the data of the first sheet
          const excellist = []; // Clear received data
          // Edit data
          for (var i = 0; i < ws.length; i++) {
            excellist.push(ws[i]);
          }
          console.log("Read results", excellist); // At this point, you get an array containing objects that need to be processed
          // Get header2-1
          const a = workbook.Sheets[workbook.SheetNames[0]];
          const headers = this.getHeader(a);
          console.log('headers', headers);
          // Get header2-2
        } catch (e) {
          return alert("Read failure!");;
        }
      };
      fileReader.readAsBinaryString(files[0]);
      var input = document.getElementById("upload");
      input.value = "";
    }
  }
}
</script>
```

### We change the form to irregular state, save and open the interface for testing

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430023301101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202004300234382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

## Step 3: implement table rendering

 ### 1. Add  ** table component**  to the interface 。


```javascript
<!-- Table components -->
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
    <!-- Table components -->
```

```javascript
data() {
    return {
      dataArr: [], // Table content data array
      // countArr: {}, // Analyze the table data and header to get a cross reference array for user-defined consolidation. For the time being, this article only writes the basis, and does not introduce the automatic consolidation of cells~~My other articles have custom merge implementation methods~
      tableColumn: [], // Table header configuration array
      tableLoading: false // Whether the table is loading
    };
  },
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035034274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035109165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430034542576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 2. Add **Table Rendering Method**

 > Note: part of the code in the table rendering method is used to map attribute names in Chinese and English. This is a function I added. Sometimes I don't need to use it. You can modify the code according to your requirements;

```javascript
setTable(headers, excellist) {
      const tableTitleData = []; // Store table header data
      const tableMapTitle = {}; // Set table content for Chinese and English
      headers.forEach((_, i) => {
        tableMapTitle[_] = "prop" + i;
        tableTitleData.push({
          prop: "prop" + i,
          label: _,
          width: 100
        });
      });
      console.log("tableTitleData", tableTitleData);
      // Mapping table content attribute name is English
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
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035152498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 3. Call  **Table Rendering Method**

```javascript
		// Add the following code to the importexcel (E) method
          // Render table1-1
          this.setTable(headers, excellist);
          // Render table1-2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430034957373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 4. Carry on function test

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020043003571776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035828289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

## Conclusion

> This code supports irregular data and can also be rendered to the interface if there is no header~~
> Welcome to point out the error of my code~  
> If there is a better way to write, welcome to come up with it and make progress together~~ 
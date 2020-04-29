# @[从0-1实现前端读取excel表格并渲染到界面](https://github.com/tangdou369098655/excelDemo/blob/master/README-zh.md)
* 本文旨在解决无需调用后端接口，实现前端读取表格文件，获取文件内容，渲染到界面的需求
* 我的其他文章可以解决扩展需求：
* 读取解析表格后执行**自动单元格合并**
* 读取解析表格后根据数据对比分析**自动设置单元格颜色**
* 读取解析表格后执行**数据分析（透析）**生成可满足用户自定义需求的**echarts**关系图
* 下载界面表格功能
## 说明
公司平时做**后台管理系统**比较多，类似需求更是十分常见，我也写过类似帖子，但是都是只放代码从来不写注释和步骤，嘿嘿，话不多说，此文章为完整的记录：
## 前提
> ###  平时我经常使用的是：
> ###  **Ant Design + Angular**
> ###  **Element UI + Vue**
> ###  **Ant Design + Vue**
> 方便起见，今天我们使用**Element UI + Vue**
基于**vue-element-admin**直接开始
## 步骤一：准备工作
### 1. [点击进入vue-element-admin下载](https://github.com/PanJiaChen/vue-admin-template)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430005909216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 2. 下载解压

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430010324823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 3. 安装依赖、运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011233719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011409967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011530155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 4. 运行成功!

[在这里插入图片描述](https://img-blog.csdnimg.cn/20200430011749527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430012004341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

## 步骤二：实现导入表格解析

 ### 1. 进入以下路径；
> src\views\dashboard\index.vue
 ### 2. 删除无用代码，准备开始；
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

 ### 3. 增加 **导入** 按钮
 

```javascript
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
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  methods: {
    /**
     * 导入表格
     */
     importExcel(e) {
      
    }
  }
}
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
```

 ### 4. 保存刷新；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430014652773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 5.  **下载xlsx** 、引入；

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430014838456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430015001318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 6. 编写**导入表格** 功能、保存刷新；

```javascript
<script>
import xlsx from "xlsx";
export default {
  name: 'Dashboard',
  methods: {
    /**
     * 导入表格
     */
     importExcel(e) {
      const files = e.target.files;
      console.log(files);
      if (!files.length) {
        return ;
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
        } catch (e) {
          return alert("读取失败!");;
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

 ### 7.  编写如下表格，用来**测试功能** ；

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020043002180586.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430021859321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430021954425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

 ### 8. 有时候，表格题目是中文，读取后我们想要得到英文属性名，那么 **增加如下代码** 、再次测试；
 

```javascript
<script>
import xlsx from "xlsx";
export default {
  name: 'Dashboard',
  methods: {
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
    /**
     * 导入表格
     */
     importExcel(e) {
      const files = e.target.files;
      console.log(files);
      if (!files.length) {
        return ;
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
          // 获取表头2-1
          const a = workbook.Sheets[workbook.SheetNames[0]];
          const headers = this.getHeader(a);
          console.log('headers', headers);
          // 获取表头2-2
        } catch (e) {
          return alert("读取失败!");;
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
### 我们把表格改成不规则状态、保存、打开界面测试

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430023301101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202004300234382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
## 步骤三：实现表格渲染

 ### 1. 界面增加 **表格组件** 。
 

```javascript
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
```

```javascript
data() {
    return {
      dataArr: [], // 表格内容数据数组
      // countArr: {}, // 分析表格数据以及表头，得到一个对照数组，用来进行自定义合并，本文暂时只写基础，不介绍自动合并单元格了哟~~我的其他文章有写自定义合并实现方法~
      tableColumn: [], // 表格表头配置数组
      tableLoading: false // 表格是否loading
    };
  },
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035034274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035109165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430034542576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
 
 ### 2. 增加 **表格渲染方法** 。

 > 备注：表格渲染方法中有一部分代码是用来映射中英文属性名的，这个是我增加的一个功能，有时候也不不需要使用，可以按自己需求来修改代码；

```javascript
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
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035152498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

### 3. 调用 **表格渲染方法** 。

```javascript
		// 在importExcel(e)方法中添加下面代码
          // 渲染表格1-1
          this.setTable(headers, excellist);
          // 渲染表格1-2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430034957373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

4. 功能测试![在这里插入图片描述](https://img-blog.csdnimg.cn/2020043003571776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430035828289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmdkb3UzNjkwOTg2NTU=,size_16,color_FFFFFF,t_70)

## 结语

> 本代码支持不规则数据，没有表头的也可以渲染到界面哦~~
> 欢迎大家指出我代码的错误~  
> 如果有更好的写法，欢迎大家提出来，共同进步哟~~ 

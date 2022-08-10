## 扩展 layui 的导出插件 layui.excel

[扩展 layui 的导出插件 layui.excel excel - layui 第三方组件平台](https://fly.layui.com/extend/excel/)





## layui admin插件使用



**第一步：从后台获取需要导出的数据**

> 一般的导出场景是后端给出获取数据的接口，前端请求后端接口后，根据接口返回参数导出，所以需要 $.ajax() 异步请求接口数据



```js
$.ajax({
    url: '/path/to/get/data',
    dataType: 'json',
    success: function(res) {
        // 假如返回的 res.data 是需要导出的列表数据
        console.log(res.data);// [{name: 'wang', age: 18}, {name: 'layui', age: 3}]
    }
});
```

**第二步：下载源码并引入插件**



```html
<!--加载插件-->
<script src="../../layui_exts/excel.js"></script>
<!--直接调用函数即可-->
<script>
    LAY_EXCEL.exportExcel([[1, 2, 3]], '表格导出.xlsx', 'xlsx')
</script>
```

**第三步：手工添加一个表头，调用导出excel的内部函数**

```js
$.ajax({
    url: '/admin/getAllTeacherJson',
    dataType: 'json',
    success: function (res) {
        // 假如返回的 res.data 是需要导出的列表数据
        console.log(res.data);// [{name: 'wang', age: 18, sex: '男'}, {name: 'layui', age: 3, sex: '女'}]
        // 1. 数组头部新增表头
        res.data.unshift({
            teacherId: 'ID', teacherNo: '教师编号', teacherPassword: '教师密码',
            teacherName: '姓名',
            teacherPhone: '电话',
            teacherClass: '班级',
            teacherPosition: '职位',
            teacherMagorName: '研究方向',
            teacherEducation: '学历',
            teacherMajorId: '专业id',
            teacherDepartmentId: '学院id',
            userStatus: '状态'
        });
        // 2. 如果需要调整顺序，请执行梳理函数
        // var data = excel.filterExportData(res.data, [
        //     'name',
        //     'sex',
        //     'age',
        // ]);
        // 3. 执行导出函数，系统会弹出弹框
        LAY_EXCEL.exportExcel({
            sheet1: res.data
        }, 'teacher_export.xlsx', 'xlsx');
    }
});
```

**执行结果：**

![image-20210513213111090](https://gitee.com/isrsy/blog-image/raw/master/img/20210513213143.png)




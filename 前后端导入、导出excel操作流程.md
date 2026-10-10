#           前后端导入、导出excel操作流程

结合easyExcel官网使用https://easyexcel.alibaba.com/docs/current/quickstart/read

## 开始准备

### 1.下载依赖包

```xml
        <!-- excel工具 -->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
        </dependency>
        <!--  excel处理工具-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>easyexcel</artifactId>
            <version>4.0.1</version>
        </dependency>
```

### 2.创建工具类ExcelUtil

将ruoyi/common/utils文件复制到自己项目，并在poi包下的ExcelUtil里加入下面方法.

```java


/**
     * 对excel表单默认第一个索引名转换成list（EasyExcel）
     *
     * @param is 输入流
     * @return 转换后集合
     */
    public List<T> importEasyExcel(InputStream is) throws Exception
    {
        return EasyExcel.read(is).head(clazz).sheet().doReadSync();
    }

    /**
     * 对list数据源将其里面的数据导入到excel表单（EasyExcel）
     *
     * @param list 导出数据集合
     * @param sheetName 工作表的名称
     * @return 结果
     */
    public void exportEasyExcel(HttpServletResponse response, List<T> list, String sheetName)
    {
        try
        {
            EasyExcel.write(response.getOutputStream(), clazz).sheet(sheetName).doWrite(list);
        }
        catch (IOException e)
        {
            log.error("导出EasyExcel异常{}", e.getMessage());
        }
    }
```

### 3.改造POJO类

```
@ExcelIgnoreUnannotated// 注解表示在导出Excel时，忽略没有被任何注解标记的字段
@ColumnWidth(16)// 注解用于设置列的宽度
@HeadRowHeight(14)// 注解用于设置表头行的高度
@HeadFontStyle(fontHeightInPoints = 11)// 注解用于设置表头的字体样式
public class Sku{

      /** 商品名称 */
    @Excel(name = "商品名称")
    @ExcelProperty("商品名称")
    private String skuName;
    
      /** 商品图片 */
    @Excel(name = "商品图片")
    @ExcelProperty("商品图片")
    private String skuImage;
}
```

### 4.看需要设置监听器（可选）

当使用 EasyExcel 读取 Excel 文件时，会逐行解析文件内容，并将每一行的数据传递给监听器进行处理。下面是一个典型的执行流程：

1. **初始化监听器**：

   - 创建一个继承自 `AnalysisEventListener` 的监听器类，重写其方法以实现特定的业务逻辑。

2. **启动解析**：

   - 使用 `EasyExcel.read()` 方法创建一个解析器实例，并传入监听器实例。
   - 调用 `sheet()` 方法指定要解析的工作表（如果文件包含多个工作表）。
   - 最后调用 `doRead()` 方法开始解析 Excel 文件。

3. **读取标题行（可选）**：

   使用AI生成代码

   - 如果 Excel 文件包含标题行，并且你希望获取这些标题信息，则可以重写 `invokeHeadMap(Map<Integer, String> headMap, AnalysisContext context)` 方法，在此方法中处理标题信息。

4. **逐行解析**：

   - 对于每一行数据，EasyExcel 会调用 `invoke(Object data, AnalysisContext context)` 方法。
   - `data` 参数是当前行的数据，根据配置可能会是不同的类型，例如 Map、List 或者 POJO。
   - `context` 参数提供了有关解析上下文的信息，包括当前行号等。
   - 在这个方法里，你可以对每一行的数据进行任何必要的操作，比如存储到数据库、进行转换或者简单地打印出来。

5. **批量处理**：

   - 默认情况下，`EasyExcel` 每读取一定数量的行（默认为1000行）就会调用一次 `invokeBatch(List<Object> datas, AnalysisContext context)` 方法。
   - 这个方法非常适合用来做批量插入数据库的操作，因为可以减少与数据库交互的次数，提高效率。

6. **完成解析**：

   - 当所有的行都被解析完毕后，`doAfterAllAnalysed(AnalysisContext context)` 方法会被调用。
   - 可以在这个方法中做一些清理工作或总结性的处理。

7. **中断解析（可选）**：

   - 如果在解析过程中遇到了特定条件想要提前结束解析，可以在 `invoke` 或其他方法中调用 `context.interrupt()` 来停止解析。

8. **异常处理**：

   - 应该始终考虑如何处理可能发生的异常，确保程序的健壮性。可以在监听器的方法中添加 try-catch 块来捕获并处理异常。

```java
public class CustomListener extends AnalysisEventListener<MyDataObject> {
    /**
     * 每隔5条存储数据库，实际使用中可以100条，然后清理list ，方便内存回收
     */
    private static final int BATCH_COUNT = 100;
    /**
     * 缓存的数据
     */
    private List<DemoData> cachedDataList = ListUtils.newArrayListWithExpectedSize(BATCH_COUNT);
    /**
     * 假设这个是一个DAO，当然有业务逻辑这个也可以是一个service。当然如果不用存储这个对象没用。
     */
    private DemoDAO demoDAO;

    public DemoDataListener() {
        // 这里是demo，所以随便new一个。实际使用如果到了spring,请使用下面的有参构造函数
        demoDAO = new DemoDAO();
    }

    /**
     * 如果使用了spring,请使用这个构造方法。每次创建Listener的时候需要把spring管理的类传进来
     *
     * @param demoDAO
     */
    public DemoDataListener(DemoDAO demoDAO) {
        this.demoDAO = demoDAO;
    }
    
    //善用AI工具
    @Override
    public void invoke(MyDataObject data, AnalysisContext context) {
        // 处理每一行数据(善用AI工具)如只有某个单元格数据的操作逻辑在下面实现
        System.out.println("Processing row " + context.readRowHolder().getRowIndex() + ": " + data);
         // 检查并处理每一行数据
        if (shouldStopReading(data)) {
            // 如果满足停止条件，则中断解析
            context.interrupt();
            return;
        }
        // 正常处理逻辑
        processRow(data);
    }
     private boolean shouldStopReading(MyDataObject data) {
        // 根据业务逻辑决定是否停止解析
        return false; // 这里只是一个示例
    }

    private void processRow(MyDataObject data) {
        // 处理单个数据对象的逻辑
    }

    @Override
    public void doAfterAllAnalysed(AnalysisContext context) {
        // 解析完成后的工作
        System.out.println("Finished processing all rows.");
    }
        /**
     * 加上存储数据库
     */
    private void saveData() {
        log.info("{}条数据，开始存储数据库！", cachedDataList.size());
        demoDAO.save(cachedDataList);
        log.info("存储数据库成功！");
    }

    // ... 其他方法 ...
}
```

![1735906228943](images/1735906228943.png)

## web导入

```java
    @PreAuthorize("@ss.hasPermi('manage:sku:add')")
    @Log(title = "商品管理", businessType = BusinessType.IMPORT)
    @PostMapping("/import")
    public AjaxResult excelImport(MultipartFile file) throws Exception {
        ExcelUtil<Sku> util = new ExcelUtil<Sku>(Sku.class);
        List<Sku> skuList = util.importEasyExcel(file.getInputStream());
        return toAjax(skuService.insertSkus(skuList));
    }
```

```vue
  <el-button plain icon="Upload" @click="handleImport" v-hasPermi="['manage:sku:add']">导入</el-button>
// 导入
const open2 = ref(false)
const uploadList = ref([]);
const uploadFileUrl = ref(import.meta.env.VITE_APP_BASE_API + "/manage/sku/import"); // 上传文件服务器地址
const headers = ref({ Authorization: "Bearer " + getToken() });
const fileList = ref([]);
const fileType=ref(["xls","xlsx",])
const fileSize=ref(100) //文件大小
const limit=ref(1) //上传数量
const isShowTip=ref(true)
const showTip = computed(
  () => isShowTip && (fileType.value || fileSize.value)
);
// 上传前校检格式和大小
function handleBeforeUpload(file) {
  // 校检文件类型
  if (fileType.value.length) {
    const fileName = file.name.split('.');
    const fileExt = fileName[fileName.length - 1];
    const isTypeOk = fileType.value.indexOf(fileExt) >= 0;
    if (!isTypeOk) {
      proxy.$modal.msgError(`文件格式不正确, 请上传${fileType.value.join("/")}格式文件!`);
      return false;
    }
  }
  // 校检文件大小
  if (fileSize.value) {
    const isLt = file.size / 1024 / 1024 < fileSize.value;
    if (!isLt) {
      proxy.$modal.msgError(`上传文件大小不能超过 ${fileSize.value} MB!`);
      return false;
    }
  }
  proxy.$modal.loading("正在上传文件，请稍候...");
  return true;
}
// 文件个数超出
function handleExceed() {
  proxy.$modal.msgError(`上传文件数量不能超过 ${limit.value} 个!`);
}

// 上传失败
function handleUploadError(err) {
  proxy.$modal.msgError("上传文件失败");
  uploadRef.value.clearFiles();
}
// 上传成功回调
function handleUploadSuccess(res, file) {
  if (res.code === 200) {
    getList();
    proxy.$modal.msgSuccess("上传excel成功");
    open2.value = false;
  } else {  
    proxy.$modal.msgError(res.msg);

  }
  uploadRef.value.clearFiles();
  proxy.$modal.closeLoading();
}
//上传
const uploadRef=ref({})
function submitUpload(){
  uploadRef.value.submit()
}
//打开上传对话框
function handleImport(params) {
  open2.value = true;
  
}
```



## web导出

```java
    @PreAuthorize("@ss.hasPermi('manage:sku:export')")
    @Log(title = "商品管理", businessType = BusinessType.EXPORT)
    @PostMapping("/export")
    public void export(HttpServletResponse response, Sku sku)
    {
        List<Sku> list = skuService.selectSkuList(sku);
        ExcelUtil<Sku> util = new ExcelUtil<Sku>(Sku.class);
        util.exportEasyExcel(response, list, "商品管理数据");
    }
```

```vue
/** 导出按钮操作 */
function handleExport() {
  proxy.download('manage/sku/export', {
    ...queryParams.value
  }, `sku_${new Date().getTime()}.xlsx`)
}
```

```javascript

// 通用下载方法
export function download(url, params, filename, config) {
  downloadLoadingInstance = ElLoading.service({ text: "正在下载数据，请稍候", background: "rgba(0, 0, 0, 0.7)", })
  return service.post(url, params, {
    transformRequest: [(params) => { return tansParams(params) }],
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    responseType: 'blob',
    ...config
  }).then(async (data) => {
    const isBlob = blobValidate(data);
    if (isBlob) {
      const blob = new Blob([data])
      saveAs(blob, filename)
    } else {
      const resText = await data.text();
      const rspObj = JSON.parse(resText);
      const errMsg = errorCode[rspObj.code] || rspObj.msg || errorCode['default']
      ElMessage.error(errMsg);
    }
    downloadLoadingInstance.close();
  }).catch((r) => {
    console.error(r)
    ElMessage.error('下载文件出现错误，请联系管理员！')
    downloadLoadingInstance.close();
  })
}

```



## 文件上传流程

1. **前端准备**：在前端页面中创建**el-upload**组件，用于选择要上传的文件，并设置action等属性以确保文件能正确发送到服务器。使用 `multipart/form-data` 格式来发送文件。
2. **API接口**：在后端定义一个接收文件上传请求的API接口。通常这个接口会接受**MultipartFile**类型的参数来处理上传的文件。
3. **文件保存**：接收到文件后，根据业务需求选择将文件**保存到本地文件系统**、**云存储服务OSS**或**数据库**中。若依框架默认配置是保存到本地文件系统。
4. **记录元数据**：保存文件的同时，在数据库中**记录有关文件的元数据信息**，例如文件名、大小、上传时间等。
5. **返回结果**：完成文件保存操作后，向客户端返回成功响应以及可能需要的额外信息（如文件访问URL）。

## 文件下载流程

返回的数据类型为二进制大对象（Blob）

1. **获取文件信息**：当用户请求下载文件时，首先从数据库或其他存储位置获取文件的相关信息。
2. **读取文件**：根据文件信息找到实际存储位置，并读取文件内容。
3. **流式传输**：通过HTTP响应输出流将文件数据逐块发送给客户端，支持大文件的下载。
4. **设置响应头**：正确设置HTTP响应头，告知浏览器如何处理即将接收的数据（例如作为附件下载）。

## MultipartFile类

MultipartFile是SpringMVC提供简化上传操作的工具类。

在不使用框架之前，都是使用原生的HttpServletRequest来接收上传的数据，文件是以二进制流传递到后端的，然后需要我们自己转换为File类。使用了MultipartFile工具类之后，我们对文件上传的操作就简便许多了。


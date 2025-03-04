### 添加注解

在使用 MultipartFile 作为传参时，我们需要在 Controller 中的方法参数前添加 `@RequestParam` 注解，并指定参数的名称。这样 Spring 框架就能够将前端传递过来的文件数据转换为 MultipartFile 对象，并注入到方法的参数中。

另外，我们还需要添加 `@RequestPart` 注解，以告诉 Spring 框架这个参数是一个文件类型的请求参数。这样 Spring 就能够正确地处理这个参数，并将文件数据转换为 MultipartFile 对象。

下面是一个使用 MultipartFile 作为传参的 Controller 方法的代码示例：

在上面的代码中，我们创建了一个名为 `uploadFile` 的方法，该方法接受一个名为 `file` 的 MultipartFile 参数。通过在参数前添加 `@RequestParam("file") @RequestPart` 注解，我们告诉 Spring 框架这个参数是一个文件类型的请求参数，并且参数的名称为 `file`。

### 类图

下面是一个类图，展示了上面代码中涉及到的几个类和注解之间的关系：

```
@RestController
public class FileUploadController {

    @PostMapping("/upload")
    public ResponseEntity<String> uploadFile(@RequestParam("file") @RequestPart MultipartFile file) {
        // 处理文件上传逻辑
        return ResponseEntity.ok("文件上传成功！");
    }
}`
```

在上面的类图中，FileUploadController 是一个 RestController，其中定义了一个 uploadFile 方法，该方法接受一个 MultipartFile 类型的参数 file。
#### 使用IDEA生成Doc的问题

在使用IDEA生成Doc文档时显示：

“F:\software\Java\jdk1.8\src.zip(java/nio/file/Path.java):106: 错误: 无法访问FileSystem
    FileSystem getFileSystem();
    ^
  错误的源文件: F:\software\Java\jdk1.8\src.zip(java/nio/file/FileSystem.java)
    文件不包含类java.nio.file.FileSystem
    请删除该文件或确保该文件位于正确的源路径子目录中。
1 个错误”

删除jdk文件中位于dk1.8\src.zip(java/nio/file中的FileSystem.java类后不再报错，成功生成。

使用命令行成功生成，没遇到上述的问题。
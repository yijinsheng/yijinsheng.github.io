- 文件按照长度排序
```shell
awk '{print length(), $0 | "sort -n" }' /path/to/text/file

```
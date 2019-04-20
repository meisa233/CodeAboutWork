### 查看文件任意几行的数据
[来源](https://blog.csdn.net/u011487593/article/details/52287991)<br />
#### 一、使用cat、tail、head组合
>
1. 查看最后1000行的数据<br />
>
```
cat filename | tail -n 1000
```
2. 查看1000到3000行的数据<br />
>
```
cat filename | head -n 3000 | tail -n +1000
```
解释如下：
```
  1、cat filename 打印文件所有内容
  2、tail -n 1000 打印文件最后1000行的数据
  3、tail -n +1000 打印文件第1000行开始以后的内容
  4、head -n 1000 打印前1000的内容
```
向某文件追加命令如下
```
cat filename | head -n 3000 | tail -n +1000 >> newfilename
```


## intellij idea 生成可执行jar 运行提示没有主清单属性
---------
- 第一步  file-->project structure 弹框后选中Atifacts---> + ---->jar---->from module with dependenceis
- 第二步  选择一个Main Class，然后指定META-INF/MANIFEST.MF的路径为src下（注意不要放到main/java目录下，否则打成的jar中META-INF/MANIFEST.MF不含有Main Class信息）
- 第三步 点击apply  ---》 OK

## jd-gui不能在jdk9上使用
介绍
JD-GUI是一个用于Java编程语言源代码“.class”文件反编译软件。使用JD-GUI中文版浏览和重建源代码的即时访问方法和字段，以代码高度方式来显示反编译过来的代码. 解决方法 
java --add-opens java.base/jdk.internal.loader=ALL-UNNAMED --add-opens jdk.zipfs/jdk.nio.zipfs=ALL-UNNAMED -jar /Applications/JD.app/Contents/Resources/Java/jd-gui-1.4.0.jar

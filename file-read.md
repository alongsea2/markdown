
1. normal read
```java
    File file = new File("F:\\program\\acg\\appsettings.config");
    FileInputStream fileInputStream = new FileInputStream(file);
    int n = 0;
    byte[] b = new byte[1024];
    int perRead = b.length;

    StringBuilder stringBuilder = new StringBuilder();
    while ( ( n = fileInputStream.read(b,0,perRead) ) != -1 ){
        stringBuilder.append(new String(b,0,n));
       
    }
```
**读取文件**
读取长度为每次read的n的长度

2.    
```java
   BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(file));
   BufferedOutputStream fileOutputStream = new BufferedOutputStream(new FileOutputStream(new File("D:\\virtualbox\\vagrant-linux_default_1461814560289_78803\\box-disk1.vmdk.bak")));

   int n2 = 0;
   byte[] b2 = new byte[1024];
   int perRead2 = b2.length;

   while ( ( n2 = bufferedInputStream.read(b2,0,perRead2) ) != -1 ){
       fileOutputStream.write(b2,0,n2);
   }
   fileOutputStream.close(); 
```
**复制文件**
buffered 更快

    
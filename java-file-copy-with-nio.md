```java
class test{
    public void func(){
        String destName =  selectDirPath + "/" + destPath;
        File destNameDir = new File(destName);
        if(dir.isDirectory()){
        File[] files = dir.listFiles();
        int fileSize = files.length;
        File zipFile;
        ByteBuffer bt = ByteBuffer.allocate(1024);
        for (File f : files) {
            if(f.exists() && f.isFile()){
                zipFile = new File(f.getPath());
                FileChannel fileInputStream = new FileInputStream(zipFile).getChannel();
                FileChannel fileOutputStream = new FileOutputStream(selectDirPath + "/"+destPath+"/" + f.getName()).getChannel();
                while (fileInputStream.read(bt) != -1){
                    bt.flip();
                    fileOutputStream.write(bt);
                    bt.compact();
                }
                bt.flip();
                while(bt.hasRemaining()){
                    fileOutputStream.write(bt);
                }
                fileInputStream.close();
                fileOutputStream.close();
                logger.info("=====>" + f.getName() + " copy ok");
            }
            }    
        }
    }
}
```
```java
Path file = Paths.get("D:\\virtualbox\\vagrant-linux_default_1461814560289_78803\\box-disk1.vmdk");
FileChannel fileChannel = FileChannel.open(file);
FileChannel writeFile = new FileOutputStream(new File("D:\\virtualbox\\vagrant-linux_default_1461814560289_78803\\box-disk1.vmdk.bak")).getChannel();

ByteBuffer byteBuffer = ByteBuffer.allocate(1024 * 1024);//读写缓存
int readN;
long b = System.currentTimeMillis();
while ( (readN = fileChannel.read(byteBuffer)) != -1 ){
    //转换模式
    byteBuffer.flip();
    //开始写
    writeFile.write(byteBuffer);
    //
    while (byteBuffer.hasRemaining()){
        writeFile.write(byteBuffer);
    }
    byteBuffer.compact();
}
writeFile.close();
fileChannel.close();
System.out.println("=====>" + (System.currentTimeMillis() - b));
```

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
````
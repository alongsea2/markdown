```java
ZipOutputStream zipOutputStream = new ZipOutputStream(new FileOutputStream(new File(zipFileName)));
        makeFileString(dir,destPath,zipOutputStream);
        for (File file : lists) {
            try {
                FileInputStream in = new FileInputStream(selectDirPath + "/" + file.getName());
                int len;
                byte[] buffer = new byte[1024];
                while ((len = in.read(buffer)) > 0) {
                    zipOutputStream.write(buffer, 0, len);
                }
            }catch (Exception e){
                logger.info("=====> pushPicOrApk error",e);
            }
}
private void makeFileString(File file,String path,ZipOutputStream zipOutputStream) throws IOException {
        if(file.isDirectory()){
            for (File file1 : file.listFiles()) {
                if(file1.getName().equals(path) && file1.isDirectory()){
                    makeFileString(file1,path + "/" + file1.getName(),zipOutputStream);
                }else{
                    if(file1.isDirectory()){
                        makeFileString(file1,path + "/" + file1.getName(),zipOutputStream);
                    }else{
                        makeFileString(file1,path,zipOutputStream);
                    }
                }
            }
        }
        if(file.isFile()){
            String fileName = new File(path,file.getName()).toString();
            logger.info("=====>" + fileName);
            zipOutputStream.putNextEntry(new ZipEntry(fileName));
        }
    }
```
递归压缩
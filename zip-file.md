```java
//压缩进压缩包的文件名字
    String path = "apks";
    //需要压缩的文件名字
    String zipFile = getFilePath() + path + ".zip";
if(appUpdateFile.listFiles().length > 0){
                        for (File file : list) {
                            String zipFilePath = file.toString();
                            zipOutputStream.putNextEntry(new ZipEntry(zipFilePath));
                            //真正需要压缩的文件路径
                            String zipFileRealPath = zipFilePath.replace(path,selectPath);
                            FileInputStream in = new FileInputStream(zipFileRealPath);
                            int len;
                            byte[] buffer = new byte[1024];
                            while ((len = in.read(buffer)) > 0) {
                                zipOutputStream.write(buffer, 0, len);
                            }
                            zipOutputStream.flush();
                        }
                        zipOutputStream.close();
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

```java
    for (File file : dir) {
                if(file.isFile()){
                    try {
                        String fileName = new File(destPath,file.getName()).toString();
                        zipOutputStream.putNextEntry(new ZipEntry(fileName));
                        FileInputStream in = new FileInputStream(file);
                        int len;
                        byte[] buffer = new byte[1024];
                        while ((len = in.read(buffer)) > 0) {
                            zipOutputStream.write(buffer, 0, len);
                        }
                        zipOutputStream.flush();
                    }catch (Exception e){
                        logger.info("=====> pushPicOrApk error",e);
                    }
                }
            }
```
非递归压缩
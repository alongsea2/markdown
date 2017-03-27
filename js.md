 (function($){
        //备份jquery的ajax方法
        var _ajax=$.ajax;

        //重写jquery的ajax方法
        $.ajax=function(opt){
            //备份opt中error和success方法
            var fn = {
                error:function(XMLHttpRequest, textStatus, errorThrown){},
                success:function(data, textStatus){}
            }
            if(opt.error){
                fn.error=opt.error;
            }
            if(opt.success){
                fn.success=opt.success;
            }

            //扩展增强处理
            var _opt = $.extend(opt,{
                error:function(XMLHttpRequest, textStatus, errorThrown){
                    //错误方法增强处理

                    fn.error(XMLHttpRequest, textStatus, errorThrown);
                },
                success:function(data, textStatus){
                    //成功回调方法增强处理
                    console.log(textStatus);
                    fn.success(data, textStatus);
                }
            });
            _ajax(_opt);
        };
    })(jQuery);



~ 是按位取反
1的二进制码
0000 0001
按位取反是
1111 1110
然后计算机存储此数
因为计算机存储存的是补码
而1111 1110
因为最高位是1
所以被按作负数求补
即符号位不变 其他位求反再加1
111 1110取反
000 0001加1
000 0010
再加上符号位就是
1000 0010
就是-2
====
If you call App.class.getResource("view/RootLayout.fxml") then the FXML file has to be in the directory: src/main/resources/mypackage/view where mypackage is the package of the class App.

If you call it with a leading slash: App.class.getResource("/view/RootLayout.fxml") then the FXML file has to be in the directory: src/main/resources/view
===========
java -jar mybatis-generator-core-1.3.2.jar -configfile ./mysql.xml -overwrite
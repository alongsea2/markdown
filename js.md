 (function($){
        //����jquery��ajax����
        var _ajax=$.ajax;

        //��дjquery��ajax����
        $.ajax=function(opt){
            //����opt��error��success����
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

            //��չ��ǿ����
            var _opt = $.extend(opt,{
                error:function(XMLHttpRequest, textStatus, errorThrown){
                    //���󷽷���ǿ����

                    fn.error(XMLHttpRequest, textStatus, errorThrown);
                },
                success:function(data, textStatus){
                    //�ɹ��ص�������ǿ����
                    console.log(textStatus);
                    fn.success(data, textStatus);
                }
            });
            _ajax(_opt);
        };
    })(jQuery);



~ �ǰ�λȡ��
1�Ķ�������
0000 0001
��λȡ����
1111 1110
Ȼ�������洢����
��Ϊ������洢����ǲ���
��1111 1110
��Ϊ���λ��1
���Ա�����������
������λ���� ����λ���ټ�1
111 1110ȡ��
000 0001��1
000 0010
�ټ��Ϸ���λ����
1000 0010
����-2
====
If you call App.class.getResource("view/RootLayout.fxml") then the FXML file has to be in the directory: src/main/resources/mypackage/view where mypackage is the package of the class App.

If you call it with a leading slash: App.class.getResource("/view/RootLayout.fxml") then the FXML file has to be in the directory: src/main/resources/view
===========
java -jar mybatis-generator-core-1.3.2.jar -configfile ./mysql.xml -overwrite
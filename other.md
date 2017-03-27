x77byc2i8y9hu4a84a039muwhi9gd9jqubeiuh9zob9yij4b
6azlisu5bk4dgilt8n8xmw9qga4dj96eeqdgxl32ecw1l7ny
froef8d9yomke4fp2poy1snwt40rs39l54yjz7qqa2nt1vym

水军表 246 猫盟


curl -X GET \
  -H "X-LC-Id: x77byc2i8y9hu4a84a039muwhi9gd9jqubeiuh9zob9yij4b" \
  -H "X-LC-Key: 6azlisu5bk4dgilt8n8xmw9qga4dj96eeqdgxl32ecw1l7ny" \
  https://leancloud.cn/1.1/rtm/transient_group/onlines?gid=569884cf34949965fe834db5

<job.config.path>F:\job.properties</job.config.path><!-- job指定服务器  -->
job.config.path=${job.config.path}

 Long beginTime = endTime-Constants.ONE_DAY_TIMESTAMP;//昨天凌晨0点;
        String res = PropertiesUtil.getValue("job.config.path");
        InputStream in = new BufferedInputStream(new FileInputStream(res));
        ResourceBundle rb = new PropertyResourceBundle(in);
        if( "node1".equals(rb.getString("live.job")) ){


D:\maven\conf\settings.xml
D:\m2\.m2





class =modeluteable slideer +width 100%
cameraCont img 100%
mongod.exe --dbpath=f://local/mongodb/data --logpath=f://mongodb/log1  --fork
./mongod --dbpath=/usr/local/mongodb/data --logpath=/usr/local/mongodb/log1 --for

db.act.aggregate([{$project:{sorts:{$multiply:["$sort",10]},iso:1}},{$sort:{"sorts":-1}},{$match:{iso:1}},{$limit:5}])

$multiply:["$sort",{$cond:{if:{$eq:["$iso",1],then:10,else:2}}}]

564a9aa32671b5ac14000001,564a9b1b2671b5ac14000002
db.act.aggregate(
       [
            {
                $project:{
                      sorts:{ //别名
                              $multiply:["$sort",{$cond:{if:{$eq:["$iso",1]},then:10,else :2}}] 
                            }
                         },
                       
                        
            },
            {
                 $sort:{"sorts":1}
                
            }       
       ]
)


function(test){
   var listData = db.act.aggregate([{
                $project:{
                      sort:{ //别名
                              $multiply:["$sorts",{
                                  $cond:{
                                      if:{$eq:["$iso",1]},      
                                      then:10,
                                      else:{
                                            $cond:{
                                                if:{
                                                   $setIsSubset:["$orgid",test]
                                                },
                                                then:2,
                                                else:1
                                              }
                                           }
                                         }
                                      }
                                 ] 
                            },
                     iso:1,
                     orgid:1,      
                   }
            },
        {$match:{"iso":0}},    
        {$sort:{"sort":1}}, 
]);
        return listData;
}





1750415096175759140  新平街388号腾飞创新园塔楼C502-3  34 5524.7 4545 32

select * from member where (university is not null or university !=0) and (province is null or province =0)

/20150916/1442367686-918.jpg,/20150916/1442367691-4.jpg,/20150916/1442367695-226.jpg,/20150916/1442367699-648.jpg,/20150916/1442367704-565.jpg,/20150916/1442367708-958.jpg

http://img.hb.aicdn.com/219bb42f423d28de345c705bf6f4efb9c59636e0417a98-tH3bZD_fw658






http://125.6.189.71/kcs/mainD2.swf?api_token=1ee0f329e0598dc4bdca852d83be306297885d07&api_starttime=1445388074664


DELIMITER $$

USE `mmeng`$$

DROP PROCEDURE IF EXISTS `p18`$$

CREATE DEFINER=`root`@`%` PROCEDURE `p18`()
BEGIN
  DECLARE stop_flag INT DEFAULT 0;
  DECLARE loop_end INT DEFAULT 0;
  DECLARE act_ids INT(11) ;
  DECLARE act_id_arr CURSOR FOR 
  SELECT 
    act_id
  FROM
    act_member_info 
  GROUP BY act_id ;
  DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET stop_flag = 1 ;
  OPEN act_id_arr ;
  FETCH act_id_arr INTO act_ids;
  WHILE
    stop_flag <> 1 DO 
    INSERT INTO temps 
	(a,b)
	SELECT 
		act_id,uid 
	FROM 
		act_member_info 
	WHERE 
		act_id = act_ids	
	LIMIT 	4;
    FETCH act_id_arr INTO act_ids;
  END WHILE ;
  CLOSE act_id_arr ;
END$$
DELIMITER ;






@RequestMapping(value="/orgattendinfo",method = RequestMethod.GET, produces = MediaTypes.JSON_UTF_8)
    public ResponseWithData orgAttendInfo(
            @RequestParam(required = false) String memberid,
            @RequestParam(required = false) String dates
    )
            throws ParseException { //todo 1*  2*

        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        SimpleDateFormat format2 = new SimpleDateFormat("yyyy-MM-dd");
        Calendar calendar = Calendar.getInstance();
        Map<String,Long> timeMap = new LinkedHashMap<String,Long>();
        List<Map> list = new ArrayList<Map>();
        String makeTime = "06:00:00";
        String nowTime;

        if(dates == null || dates.equals("")){
            int a= calendar.get(Calendar.YEAR);
            int b = calendar.get(Calendar.MONTH)+1;
            int c = calendar.get(Calendar.DATE);
            nowTime = a+"-"+b+"-"+c+" "+makeTime;
        }else{
            nowTime = dates+" "+makeTime;
        }


        long todaySixTimestamp = format.parse(nowTime).getTime();//dates凌晨六点的时间戳
        long nowTimestamp = System.currentTimeMillis();//现在的时间戳
        //if(nowTimestamp < todaySixTimestamp){//当前时间小于今天凌晨六点的时间
            for(int i=1;i<7;i++){
                Long x = orgAttentionMemberService.getAttendCountByTime(todaySixTimestamp - i * 86400000, todaySixTimestamp-(i-1)*86400000, McOrgApi.orgId);
                System.out.println("time:"+format2.format( new Date(todaySixTimestamp-i*86400000))+":"+x);
                timeMap.put(format2.format(new Date(todaySixTimestamp - i * 86400000)), x);
                /*
                    db.find({
                      "createTime":{
                        $lte:todaySixTimestamp-i*86400000,//昨天的凌晨六点
                        $gt:todaySixTimestamp-(i+1)*86400000,//前天的凌晨六点
                      }
                    })

                 */
            }
            /*今天的事实数据
            db.find({
                    "createTime":{
                        $lte:nowTimestamp-86400000,
                        $gte:todaySixTimestamp
            }
            })
            */
        //}else{//当前时间大于今天凌晨六点的时间

            /*for(int i=1;i<7;i++){
                Long x = orgAttentionMemberService.getAttendCountByTime(todaySixTimestamp - i * 86400000, todaySixTimestamp-(i-1)*86400000, McOrgApi.orgId);
                System.out.println("time:"+format.format( new Date(todaySixTimestamp-i*86400000))+":"+x);
                timeMap.put(format.format(new Date(todaySixTimestamp-i*86400000)),x);*/
                /*
                    db.find({
                      "createTime":{
                        $lte:todaySixTimestamp,//今天的凌晨六点
                        $gte:todaySixTimestamp-i*86400000,//昨天的凌晨六点
                      }
                    })

            }*/
            /*今天的事实数据
            db.find({
                    "createTime":{
                        $lte:nowTimestamp,
                        $gte:todaySixTimestamp
            }
            })
            */

        //}
        list.add(timeMap);
        return new ResponseWithData(1,list);


db.getCollection('MM_STAR').update({"isSign":"N"},{$set:{"orgType":"P"}},true,true)
db.getCollection('MM_STAR').update({"isSign":"B"},{$set:{"orgType":"C","isSign":"Y"}},true,true)
db.getCollection('MM_STAR').update({"isSign":"P"},{$set:{"orgType":"P","isSign":"Y"}},true,true)


{
    "_id" : ObjectId("56af208e07e60827af3fbbf9"),
    "actionType" : "invite",
    "actionTypeName" : "邀请他人加入猫盟",
    "goodsNum" : 2.0000000000000000,
    "status" : "A",
    "goodsType" : "C",
    "todayGoodsNumMax" : 100.0000000000000000,
    "from" : "M"
}

/* 28 */
{
    "_id" : ObjectId("56af210507e60827af3fbbfa"),
    "actionType" : "beinvited",
    "actionTypeName" : "邀请码加入猫盟",
    "goodsNum" : 1.0000000000000000,
    "status" : "A",
    "goodsType" : "C",
    "todayGoodsNumMax" : 100.0000000000000000,
    "from" : "M"
}



 /*public static void main(String[] args) throws IllegalAccessException, InstantiationException, IntrospectionException, InvocationTargetException {
        Field[] com = Competitor.class.getDeclaredFields();
        for (Field field : com) {
            PropertyDescriptor pd = new PropertyDescriptor(field.getName(),Competitor.class);
            //获得set方法
            Method method = pd.getWriteMethod();
            method.invoke(Competitor.class,new Object[]{"123"});
            //获得get方法
            Method get = pd.getReadMethod();
            Object getValue = get.invoke(Competitor.class, new Object[]{});
            System.out.println("field:" + field.getName() + "---getValue:" + getValue);
        }
    }*/


db.collection.aggregate([

{$match:{status:1}},

{$group:{_id:"$name",count:{$sum:1}}},

{$match:{count:1}}

]);



D:\maven\conf\settings.xml
D:\m2\.m2
java -jar swagger2markup-cli-1.1.0.jar convert -i "http://192.168.20.61:8080/yqb-api/api-docs/" -d F:\

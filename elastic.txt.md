                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                ry":{
        "bool":{
            "must":{
                "match":{"id":3260583}
            }
        }
    }
}'
curl http://192.168.10.223:9200/prod_location/anchor/1 -d '
{

 "anchorId":"400009","location":{"lat":4.2,"lon":5.2}


}'
{
   "anchor":{
       "properties":{
           "location":{
               "type":"geo_point","store":true
           },
           "anchorId":{"type":"string","store":true}
       }
   }
}


==================================================
1.创建 index和type http://192.168.10.223:9200/test_location/
2.建立映射 http://192.168.10.223:9200/test_location/_mapping/anchor 
{
    "anchor":{
        "properties":{
            "anchorId":{"type":"string","store":true},
            "location":{"type":"geo_point","store":true},
            "status":{"type":"integer","store":true}
        }
    }
}
3.测试数据 http://192.168.10.223:9200/test_location/anchor/400072 
{
    "anchorId":"400072",
    "location":{
        "lat":12.123,
        "lon":111.456
    },
    "status":0
}
{
    "anchorId":"400072",
    "location":{
        "lat":12.123,
        "lon":111.456
    },
    "status":1
}
4.测试数据{
        "script_fields": {
            "distance": {
                "params": {
                    "lat": 12.22,
                    "lon": 120.22
                },
                "script": "doc['location'].distanceInKm(lat,lon)"
            }
        },
        "sort": {
            "_script": {
                "type": "number",
                "lang" : "groovy",
                "script": {
                    "params": { 
                        "lat": 1.22,
                        "lon": 11.22
                    },
                    "inline": "if(_source.status == 0){doc['location'].distanceInKm(lat,lon) + 100 }else{doc['location'].distanceInKm(lat,lon) - 100}"
                },
                "order": "asc"
            }
        },
        "from": 0,
        "size": 5
}




{
        "script_fields": {
            "anchorId":{"script":"_source.anchorId"},
            "distance": {
                "params": {
                    "lat": 122.2,
                    "lon": 132222222.222
                },
                "script": "doc['location'].distanceInKm(lat,lon)"
            }
        },
        "sort": {
            "_script": {
                "type": "number",
                "lang" : "groovy",
                "script": {
                    "params": { 
                        "lat": 122.2,
                        "lon": 132222222.222
                    },
                    "inline": "if(_source.status == 0){doc['location'].distanceInKm(lat,lon) + 500000 }else{doc['location'].distanceInKm(lat,lon)- 500000}"
                },
                "order": "asc"
            }
        },
        "from": 0,
        "size": 5
}

====================
{
    "query": {
        "bool": {
            "must": {
                "match": {
                    "anchorId": "401027"
                }
            }
        }
    },
    "aggs": {
        "by_type": {
            "terms": {
                "field": "memberId",
                "size": 0
            },
            "aggs": {
                "sums": {
                    "sum": {
                        "field": "calculateSum"
                    }
                }
            }
        }
    },
    "size": 0
}
==========================================================================
{
    "query": {
        "bool": {
            "must": {
                "match": {
                    "anchorId": "401027"
                }
            }
        }
    },
    "aggs": {
        "by_type": {
            "terms": {
                "field": "anchorId",
                "size": 0
            },
            "aggs": {
                "by_type_2": {
                    "terms": {
                        "field": "memberId",
                        "size": 0
                    },
                    "aggs": {
                        "sums": {
                            "sum": {
                                "field": "calculateSum"
                            }
                        }
                    }
                }
            }
        }
    },
    "size": 0
}
===========================================================================================================================
{
    "query": {
        "bool": {
            "must":[
                {
                    "match": {"anchorId": "2292"}
                },
                {
                    "match":{"calculateUnit":"C"}
                }
            ]
        }
    },
    "aggs": {
        "by_type": {
            "terms": {
                "field": "memberId",
                "order":{"sums":"desc"},
                "size": 0
            },
            "aggs": {
                    "sums": {
                        "sum": {
                            "field": "calculateSum"
                        }
                    }
                }
        }
    } ,
    
    "size":0
}
============================================
{
    "query": {
        "bool": {
            "must":[
                {
                    "match": {"anchorId": "2292"}
                },
                {
                    "match":{"calculateUnit":"C"}
                }
            ]
        }
    },
    "aggs": {
        "by_type": {
            "terms": {
                "field": "memberId",
                "order":{"calculateSum":"desc"},
                "size": 0
            },
            "aggs": {
                    "calculateSum": {
                        "sum": {
                            "field": "calculateSum"
                        }
                    }
                }
        }
    }
}
=======================
curl -XPOST http://10.168.154.195:9200/anchor_live_room_stage/record/_search?pretty -d '
{
    "sort": [
        {"liveStatus": "asc"},
        {"openTime": "desc"},
        {"stopTime": "desc"}
        ]
}'


{
    "mappings": {
        "record": {
            "properties": {
                "sex": {
                    "type": "string",
                    "index": "not_analyzed"
                },
                "universityId":{
                    "type":"string",
                    "index":"not_analyzed"
                }
            }
        }
    }
}'
curl -XPOST http://10.168.154.195:9200/anchor_live_room_stage/record/_search -d '
{
    "query": {
        "bool": {
            "should": [{
                "term": {
                    "sex": {
                        "value": "M",
                        "boost": 3
                    }
                }
            },{
                "term":{
                    "sex":{
                        "value":"W",
                        "boost":0
                    }
                }
            }
            ]
        }
    },
    "sort": [
        {"liveStatus": "asc"},
        {"sort": "desc"},
        {"_score":"desc"},
        {"peopleCount": "desc"},
        {"stopTime": "desc"}
        ]
}
'

curl -XPOST http://10.168.154.195:9200/anchor_live_room_stage/record/_search?pretty -d '
{
    "query": {
        "bool": {
            "should": [
                {
                    "regexp": {
                        "universityId": {
                            "value": "838",
                            "boost": 2
                        }
                    }
                },
                {
                    "regexp": {
                        "universityId": {
                            "value": ".*",
                            "boost": 0
                        }
                    }
                }
            ]
        }
    },
    "sort": [
        {
            "liveStatus": "asc"
        },
        {
            "_score": "desc"
        },
        {
            "openTime": "desc"
        },
        {
            "stopTime": "desc"
        }
    ]
}'
=========================================
{
    "query": {
        "bool": {
            "must": {
                "match": {
                    "anchorId": "401027"
                }
            }
        }
    }
}
========================================
public static void main(String[] args) throws IOException, JsonProcessingException, ExecutionException, InterruptedException {

        /*QueryBuilder query = QueryBuilders.matchQuery("anchorId","2292");
		QueryBuilder aQuery = QueryBuilders.matchQuery("calculateUnit","C");
		//QueryBuilder anotherQuery = QueryBuilders.matchQuery("giftType","kiss");
		//QueryBuilder queryBuilder = boolQuery().must(anotherQuery).must(query);

		AbstractAggregationBuilder agg = AggregationBuilders.sum("giftNum").field("giftNum");

		SearchResponse aggResult = el.search("anchor_index_qa", "gift_record", queryBuilder,agg);
		//Sum r = aggResult.getAggregations().get("giftNum");

		//System.out.println(r.getValue());

		QueryBuilder queryBuilder = QueryBuilders.boolQuery().must(query).must(aQuery);


		AbstractAggregationBuilder termsBuilder = AggregationBuilders.terms("by_type")
				.field("memberId")
				.size(0)
				.order(Terms.Order.aggregation("calculateSum",false))
				.subAggregation(AggregationBuilders.sum("calculateSum").field("calculateSum"));

		SearchResponse res = el.search("anchor_index_qa", "gift_record", queryBuilder, termsBuilder);

		Terms terms = res.getAggregations().get("by_type");
		System.out.println(terms.getBuckets().get(0).getKeyAsString());

		/*Map<String,Object> template_params = new HashMap<String,Object>();

		template_params.put("lat",12.22);
		template_params.put("lon",120.22);

        Map<String,Object> template_params2 = new HashMap<String,Object>();
        template_params2.put("lat",1.22);
        template_params2.put("lon",11.22);
        SortBuilder sortBuilder = SortBuilders.scriptSort(new Script("if( doc['status'] == [0]){doc['location'].distanceInKm(lat,lon) + 100 }else{doc['location'].distanceInKm(lat,lon) - 100}", ScriptService.ScriptType.INLINE, "groovy", template_params), "number").order(SortOrder.ASC);
        SearchResponse s = client.prepareSearch("anchor_location_index_dev").setTypes("location").
                addScriptField("distance",new Script("doc['location'].distanceInKm(lat,lon)",ScriptService.ScriptType.INLINE, "groovy", template_params2)).
                addScriptField("anchorId",new Script("_source.anchorId")).
                addSort(sortBuilder).
                setFrom(0).
                setSize(5).
                execute().
                get();
        System.out.println(s.toString());

        for (SearchHit searchHitFields : s.getHits()) {
            Map<String, SearchHitField> fields = searchHitFields.getFields();
            System.out.println(fields.get("anchorId").getValue());
        }*/

		/*QueryBuilder anotherQuery = QueryBuilders.matchQuery("calculateUnit","C");
		QueryBuilder anotherQuery2= QueryBuilders.matchQuery("orgId","57206223e4b0d7200c055236");
		//QueryBuilder rangeQuery = QueryBuilders.rangeQuery("createTime").gt(1359440000000L).lte(1473414400000L);
		QueryBuilder query = boolQuery().must(anotherQuery).must(anotherQuery2);//.must(rangeQuery);

		AbstractAggregationBuilder agg = AggregationBuilders.sum("calculateSum").field("calculateSum");

		SearchResponse sea = el.search("anchor_index_dev", "gift_record", query,agg);

		Sum r = sea.getAggregations().get("calculateSum");

		System.out.println(r.getValue() / 1000 + ":" + r.getName());


		Map<String,String> map = new HashMap<String, String>();
		map.put("elaTest1","1");
		map.put("elaTest2","2");
		map.put("elaTest3","3");
		map.put("elaTest4","5");

		ObjectMapper mapper = new ObjectMapper();
		String s = mapper.writeValueAsString(map);
		String index = "testindex";
		//elaClient.addDocument("testindex","test","testid",s);


        GetResponse ss = elaClient.getDocumnet(index, "test", "testid");
        System.out.println(ss.getSourceAsString());

		/**//*ElasticsearchClient de = new ElasticsearchClient();
        String index = "customer";
		String type = "external";

		*//**//*Demo 1: create document*//**//*
		*//**//*
			XContentBuilder builder = jsonBuilder().startObject().field("name", "kimchy").field("age", 44).endObject();


			de.addDocument(index, type, builder.string());

		 * *//**//*

		*//**//*Demo 2: create documents*//**//*
		*//**//*List<String> jsons = new ArrayList<String>();
		for(int i=0;i<10;i++){
			jsons.add(jsonBuilder().startObject().field("name", "kimchy-"+i).field("age", 44).endObject().string());
		}

		de.addDocuments(index, type, jsons);*//**//*


		*//**//*Demo 3: search documents*//**//*

		//QueryBuilder qb = QueryBuilders.termQuery("name", "kimchy");

		QueryBuilder query = QueryBuilders.matchQuery("name", "kimchy"); //name contain 'kimchy'

		QueryBuilder filter  = null;
		filter = QueryBuilders.rangeQuery("age").lt(40); //age <40

		long start = System.currentTimeMillis();
		SearchHits hits =de.search(index, type, query, filter, 0 , 20);
		long end = System.currentTimeMillis();
		System.out.println(end - start);

		SearchHit[] array = hits.getHits();
		if(array!=null && array.length!=0){
			for(SearchHit hit : array){
				System.out.println(hit.getSource().get("name") + " , " + hit.getSource().get("age"));
			}
		}
		de.close();
		*/
	}
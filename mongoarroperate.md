 + package com.xlingmao.member.dao.impl;
 + 
 + import com.xlingmao.maomeng.entity.mongodb.Friends;
 + import com.xlingmao.maomeng.entity.mongodb.MemberFriend;
 + import com.xlingmao.member.dao.MemberFriendDao;
 + import org.springframework.beans.factory.annotation.Autowired;
 + import org.springframework.data.domain.Sort;
 + import org.springframework.data.mongodb.core.MongoTemplate;
 + import org.springframework.data.mongodb.core.aggregation.Aggregation;
 + import org.springframework.data.mongodb.core.aggregation.AggregationResults;
 + import org.springframework.data.mongodb.core.query.Criteria;
 + import org.springframework.data.mongodb.core.query.Query;
 + import org.springframework.data.mongodb.core.query.Update;
 + import org.springframework.stereotype.Repository;
 + 
 + import java.util.List;
 + 
 + import static org.springframework.data.mongodb.core.aggregation.Aggregation.*;
 + 
 + /**
 +  * Created by dell on 2016/5/29.
 +  */
 + @Repository
 + public class MemberFriendDaoImpl implements MemberFriendDao{
 + 
 +     @Autowired
 +     MongoTemplate mongoTemplate;
 + 
 + 
 +     @Override
 +     public List<Friends> getMemberFriends(String memberId,int pageNo,int pageSize) {
 +         Aggregation agg = newAggregation(
 +                 unwind("friends"),
 +                 match(Criteria.where("memberId").is(memberId)),
 +                 sort(Sort.Direction.DESC,"friends.createTime"),
 +                 project().andInclude("test.memberId","test.createTime").andExclude("_id"),
 +                 skip(pageNo),
 +                 limit(pageSize)
 +         );
 +         AggregationResults<Friends> res = mongoTemplate.aggregate(agg, Friends.class, Friends.class);
 +         return res == null ? null : res.getMappedResults();
 +     }
 + 
 +     @Override
 +     public void saveFriend(String friendId, String memberId, String actionType) {
 +         Query query = new Query();
 +         query.addCriteria(Criteria.where("memberId").is(memberId));
 +         Update update = new Update();
 + 
 +         if(actionType == null || "add".equals(actionType) ){
 +             long now = System.currentTimeMillis();
 +             Friends friends = new Friends();
 +             friends.setFriendId(friendId);
 +             friends.setCreateTime(now);
 +             update.push("friends",friends);
 +         }else if("del".equals(actionType)){
 +             update.pull("friends.friendId",friendId);
 +         }
 +         mongoTemplate.updateFirst(query,update, MemberFriend.class);
 +     }
 + 
 +     @Override
 +     public long countMemberFriendNums(String memberId) {
 +         Aggregation agg = Aggregation.newAggregation(
 +                 match(Criteria.where("memberId").is(memberId)),
 +                 project().and("friends").project("size").as("count")
 +         );
 +         AggregationResults<Long> res = mongoTemplate.aggregate(agg,MemberFriend.class,long.class);
 +         return res == null ? 0 : res.getMappedResults().get(0);
	}
 + }

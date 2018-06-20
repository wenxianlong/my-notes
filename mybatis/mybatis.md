### mybatis中会遇到id在某个list集合中，sql语句如下: 其中idList是传递的参数类似[1,2,3]

```
select * from user where id in   
<foreach collection="idList" index="index" item="id" open="(" separator="," close=")">  
        #{id}  
</foreach>  

```

### mybatis中有时会要求位数匹配的功能，使用``` RIGHT ```获取尾数; 比如订单尾数为1和2的,订单1001和10002满足

```
SELECT RIGHT(order_id,1) AS tail, id FROM jd_nethp_manual_triage

```

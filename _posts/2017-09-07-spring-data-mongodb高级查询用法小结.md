---
layout: default
title: springboot实现监听redis key失效事件
---

### 0.准备工作，先介绍下mongodb的集合名，java bean，以及数据结构
* mongodb集合名称：doc_pageview
* 集合数据结构：
	
	```
	{
	"_id" : ObjectId("xxxx"),    
	"shopId" : "xxx",
	"domain" : "xxxx",
	"url" : "xxxx",
	"pageTitle" : "xxx",
	"pageId" : "xxxx",
	"cookie" : "xxx",
	"ip" : "xxxx",
	"viewDate" : NumberLong(xxxx),
	"date" : "yyyy-MM-dd",
	"stayTime" : NumberLong(xxxx)
	}
	```
* java bean(为保证代码简洁，@Getter、@Setter的注解，使用到lombok)：

	```
	 @Document(collection = "doc_pageview")
	 @Getter
	 @Setter
	public class PageViewBean {
	    @Id
	    private String id; //主键
	    private String domain; //域名
	    private String url; //相对链接
	    private String pageTitle; //页面标题
	    private String pageId; //页面ID
	    private String cookie;
	    private Long viewDate; //访问时间（时间戳）
	    private String shopId; //店铺ID
	    private String ip; //访问IP
	    private String date; // 日期：yyyy-MM-dd
	    private Long stayTime; // 停留时间
	}
	```

### 1.相关包的引入（笔者使用版本是1.4.0.RELEASE，读者可以选择更高版本）
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
    <version>1.4.0.RELEASE</version>
</dependency>
```
### 2.分页、排序操作
分页排序操作使用org.springframework.data.domain.Pageable，如果根据条件查询，使用注解@Query（org.springframework.data.mongodb.repository.Query）即可。eg：查询cookie=111，date=2017-09-07的前100条记录，按照viewDate倒序（注意实现 MongoRepository接口）

```
public interface PageViewLogRepository extends MongoRepository<PageViewLogBean, String> {

    /**
     * 根据参数查询日志集合
     * @param cookie
     * @param date
     * @param pageable
     * @return
     */
    @Query("{'cookie':?0,'date':?1}")
    List<PageViewLogBean>  findByCookieAndDate(String cookie,String date, Pageable pageable);
}       
```

测试：

```
    Pageable pageable = new PageRequest(0, 100, new Sort(Sort.Direction.DESC, "viewDate"));
    Page<PageViewLogBean> page = pageViewLogRepository. findByCookieAndDate("111", "2017-09-07", pageable);
```

### 3. 聚合操作（分组、求和、平均值、分页、排序）
刚开始尝试使用上面的@Query来做，后来没有发现解决办法（个人认为@Query只是用来处理比较简答的查询，大家如果有，欢迎提出来）;本着关系型数据库能做的，mongodb也能做的指导思想，于是尝试新的api；虽然花了点时间，最终还是找到了，核心是org.springframework.data.mongodb.core.aggregation里面的类。其中，Aggregation.newAggregation这个方法是Aggregation的静态方法，支持可变参数，源码如下：

```
	/**
	 * Creates a new {@link Aggregation} from the given {@link AggregationOperation}s.
	 *
	 * @param operations must not be {@literal null} or empty.
	 */
	public static Aggregation newAggregation(AggregationOperation... operations) {
		return new Aggregation(operations);
	}
	
```
AggregationOperation是一个接口，很多聚合操作只需实现该接口即可,以下是所有的实现类。
![](https://static.oschina.net/uploads/img/201709/07172145_Unoc.png)
可以猜到SortOperation（排序）、LimitOperation和SkipOperation（分页）；好了，现在回到具体问题上面，eg：查询从2017-08-07到2017-09-07一个月店铺的平均单个页面停留时间（总的停留时间/总的pv数），并按从大到小的顺序排序（貌似没啥具体意义，😁😁😁）

```
import org.springframework.data.domain.Sort;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.aggregation.*;
import org.springframework.data.mongodb.core.query.Criteria;

// 根据avgPvTime=sumStayTime/countPv排序，如下
SortOperation sortOperation = sortOperation = Aggregation.sort(new Sort(Sort.Direction.DESC, "avgPvTime"));

// 根据shopId group查询
Aggregation aggregation = Aggregation.newAggregation(
                            Aggregation.match(Criteria.where("date").gte("2017-08-07").lte("2017-09-07")),
                            Aggregation.group("shopId").sum("stayTime").as("sumStayTime").count().as("countPv"),
                            new ProjectionOperation().andInclude("sumStayTime", "countPv").and("sumStayTime").divide("countPv").as("avgPvTime"),
                            sortOperation,
                            Aggregation.skip(0),
                            Aggregation.limit(100));
```

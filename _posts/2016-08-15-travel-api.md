##接口文档：
####一. 课程接口
* 获取四大教材下面章节
  * url: http://www.beangou.com:9090/app/v1/courses
  * method: post
  * params: {"parentId": 教材id}
    * 《全国导游基础知识》 => id = 1
    * 《政策法律与法规》  => id = 2
    * 《导游业务》   => id = 3
    * 《景点介绍》 => id = 4
  * return:   
    <code><pre>
    {
  "data": [
    {
      "id": 29,
      "type": 2,
      "title": "第一章 导游活动与旅游业",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "sonCourses": null
    },
    {
      "id": 30,
      "type": 2,
      "title": "第二章 中国历史文化",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "sonCourses": null
    },
    {
      "id": 36,
      "type": 2,
      "title": "第三章 中国旅游景观",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "sonCourses": null
    },
    {
      "id": 37,
      "type": 2,
      "title": "第四章 中国的名族与名俗",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "sonCourses": null
    }
  ],
  "responseCode": "00",
  "responseMsg": "SUCCESS"
}
    </code></pre>
    
* 获取节以及子节标题
  * url: http://www.beangou.com:9090/app/v1/courses
  * method: post
  * params: {"parentId": 章id, type="3"} 
    * type = 1 => 书
    * type =2 => 章
    * type =3 => 节
    * type =4 => 子节
  * return:     
    <code><pre><div>
    	{
  "data": [
    {
      "id": 31,
      "type": 3,
      "title": "第一节 旅游活动及其分类",
      "content": null,
      "parentId": 29,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "sonCourses": [
        {
          "id": 34,
          "type": 4,
          "title": "一、旅游活动",
          "content": null,
          "parentId": 31,
          "createdAt": null,
          "updatedAt": null,
          "deletedAt": null,
          "sonCourses": null
        },
        {
          "id": 35,
          "type": 4,
          "title": "二、旅游活动的类型",
          "content": null,
          "parentId": 31,
          "createdAt": null,
          "updatedAt": null,
          "deletedAt": null,
          "sonCourses": null
        },
        {
          "id": 40,
          "type": 4,
          "title": "三、旅游活动的特点",
          "content": null,
          "parentId": 31,
          "createdAt": null,
          "updatedAt": null,
          "deletedAt": null,
          "sonCourses": null
        }
      ]
    },
    {
      "id": 32,
      "type": 3,
      "title": "第二节 旅游活动的主体与客体",
      "content": null,
      "parentId": 29,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "sonCourses": [
        {
          "id": 41,
          "type": 4,
          "title": "一、旅游活动的主体",
          "content": null,
          "parentId": 32,
          "createdAt": null,
          "updatedAt": null,
          "deletedAt": null,
          "sonCourses": null
        },
        {
          "id": 43,
          "type": 4,
          "title": "二、旅游活动的客体",
          "content": null,
          "parentId": 32,
          "createdAt": null,
          "updatedAt": null,
          "deletedAt": null,
          "sonCourses": null
        }
      ]
    }
  ],
  "responseCode": "00",
  "responseMsg": "SUCCESS"
}		
    			
   </div> 			
    </code></pre>
    
 * 获取小节内容
    * url: http://www.beangou.com:9090/app/v1/course
    * method: post
    * params: {"id": 小节id}    
    * return:     
    <code><pre>
    	{
  "data": {
    "id": 34,
    "type": 4,
    "title": "一、旅游活动",
    "content": "<p>（一）<strong>旅游的概念 根据世界旅游组织的定义，旅游是指。。。。 </strong></p><p>（二）<span style=\"color: rgb(184, 49, 47);\">旅游活动的基本特征</span></p><p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;1. 旅行与逗留的合成性 旅游活动是由旅行活动。。。。&nbsp;</p><p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;2. 异地性&nbsp;</p><p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;3. 暂时性</p><p><br></p><p><img class=\"fr-dib fr-draggable\" src=\"http://pic2.ooopic.com/12/28/23/95bOOOPIC20_1024.jpg\" style=\"width: 500px;\"></p><p><br></p>",
    "parentId": 31,
    "createdAt": null,
    "updatedAt": null,
    "deletedAt": null,
    "sonCourses": null
  },
  "responseCode": "00",
  "responseMsg": "SUCCESS"
}	
    </code></pre>
    
    

####二. 题目接口
* 根据书本id获取所有章节，以及下面的题目类型
  * url: http://www.beangou.com:9090/app/v1/getChapters
  * method: post
  * params: {"parentId": 书名id} 
  * 返回值中的typeList表示该章有哪几种题目类型（1：单选、2：多选、3：判断）   
  * return:     
    <code><pre>
    	{
  "data": [
    {
      "id": 29,
      "type": 2,
      "title": "第一章 导游活动与旅游业",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "typeList": [
        1,
        2,
        3
      ],
      "questions": null,
      "sonCourses": null
    },
    {
      "id": 30,
      "type": 2,
      "title": "第二章 中国历史文化",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "typeList": [],
      "questions": [],
      "sonCourses": null
    },
    {
      "id": 36,
      "type": 2,
      "title": "第三章 中国旅游景观",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "typeList": [],
      "questions": [],
      "sonCourses": null
    },
    {
      "id": 37,
      "type": 2,
      "title": "第四章 中国的名族与名俗",
      "content": null,
      "parentId": 1,
      "createdAt": null,
      "updatedAt": null,
      "deletedAt": null,
      "typeList": [],
      "questions": [],
      "sonCourses": null
    }
  ],
  "responseCode": "00",
  "responseMsg": "SUCCESS"
}
    </code></pre>
    
* 根据章id、题目类型获取题目列表
  * url: http://www.beangou.com:9090/app/v1/questionsByCourseAndType
  * method: post
  * params: {"courseId": "章id", "type": "题目类型"}
  * 传参type（1：单选、2：多选、3：判断） 
  * 判断题： answer：1 表示争取， 0表示错误  
  * return:     
    <code><pre>
    	{
  "data": [
    {
      "id": 1,
      "title": "新中国成立时间 是1929年",
      "options": [],
      "answers": "\"2\"",
      "analysis": "是1949年",
      "type": 3,
      "createdAt": 1469956567977,
      "updatedAt": 1469956567977,
      "deletedAt": null
    },
    {
      "id": 2,
      "title": "汉代第一个皇帝是汉武帝",
      "options": [],
      "answers": "\"0\"",
      "analysis": "汉高祖",
      "type": 3,
      "createdAt": 1469956674263,
      "updatedAt": 1469956674263,
      "deletedAt": null
    },
    {
      "id": 3,
      "title": "djskfjk ",
      "options": [],
      "answers": "\"1\"",
      "analysis": "dsfdjks ",
      "type": 3,
      "createdAt": 1469957053649,
      "updatedAt": 1469957053649,
      "deletedAt": null
    }
  ],
  "responseCode": "00",
  "responseMsg": "SUCCESS"
}
    </code></pre>
    

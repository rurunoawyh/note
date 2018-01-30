##maven 配置

###配置释义
`<moudelVersion>` 模型版本

`<groupId>`  公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.winner.trade，maven会将该项目打成的jar包放本地路径：/com/winner/trade

`<artfactid>` 本项目的唯一id 用于区分groupId
 
###repository和mirrors区别
1.repository(仓库)

   ![](img/maven-1.png)
   
1.1 Maven仓库主要有2种：

    分两种remote repository 和 local repository
 
1.2 remote repository 主要分三种
     
     中央仓库 http://repo1.maven.org/maven2/ 
     私服 内网建的repository
     其他公共仓库
     
###  mybatis报错The error occurred while setting parameters
    解决方法：
    1.检查sql语句，最好的检查方法就是将sql语句复制到查询器中执行一遍。 
    2.检查Mapper接口，参数名一定要对上！！！ 
    3.检查字段是否出现sql关键字！（比如call,desc），这个很重要，因为call在java中并不是关键字，但是在sql中是关键字！ 
    4.写完后一定要记得commit（）
    
###  org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)
     解决方法：
     1.检查xml文件所在package名称是否和Mapper interface所在的包名
     
     <mapper namespace="me.tspace.pm.dao.UserDao">
      mapper的namespace写的不对！！！注意系修改。
     
     2.UserDao的方法在UserDao.xml中没有，然后执行UserDao的方法会报此
     
     3. UserDao的方法返回值是List<User>,而select元素没有正确配置ResultMap,或者只配置ResultType!
     
     4. 如果你确认没有以上问题,请任意修改下对应的xml文件,比如删除一个空行,保存.问题解决
     
     5.看下mapper的XML配置路径是否正确
      mybatis.type-aliases-package=com.alibaba.bizmodel.dal.domain
      mybatis.mapper-locations=classpath*:/mapper/*.xml

   
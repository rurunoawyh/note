##maven 私服搭建

 ####初始化
     安装jdk、maven、nexus
 
 ####安装jar
 
 1. 手动上传
      
 2. maven配置上传、
 
 
    需要修改2个地方，第一是在settings文件的servers节点下加入，这是配置私服的密码和仓库，配合pom文件中的仓库地址就构成了完成的访问私服的要素，帐号和密码之所以在settings中设置，是由于settings文件是本地的，而pom.xml文件是公共的，不安全，所以放在settings中：
        <server>
          <id>nexus-releases</id>
          <username>admin</username>
          <password>admin123</password>
        </server>
    
        <server>
            <id>nexus-snapshots</id>
            <username>admin</username>
            <password>admin123</password>
        </server> 
        
        第二是在pom.xml中增加，下面的id和settings中的id必须要一样。
        
        <distributionManagement>
                <repository>
                    <id>nexus-release</id>
                    <name>Nexus Release Repository</name>
                    <url>http://localhost:8081/nexus/content/repositories/release/</url>
                </repository>
                <snapshotRepository>
                    <id>nexus-snapshots</id>
                    <name>Nexus Snapshot Repository</name>
                    <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
                </snapshotRepository>
            </distributionManagement>
            
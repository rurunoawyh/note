###logback的介绍
    Logback是log4j的创始人另一个开源组件。分为下面几个模块：logback-core、logback-classic、logback-access。
###logback的配置介绍
   * Logger、appender和layout
      
         logger：作为日志的记录器，把它关联到应用的对用context上，用于存放日志对象，也可以定义日志类型、级别。
         appender:要用于指定日志输出的目的地，目的地可以是控制台、文件、远程套接字服务器、 MySQL、 PostreSQL、 Oracle和其他数据库、 JMS和远程UNIX Syslog守护进程等。
         layout:负责把事件转换成字符串，格式化的日志信息的输出。
         
         logger context:各个logger 都被关联到一个 LoggerContext，LoggerContext负责制造logger，也负责以树结构排列各 logger。其他所有logger也通过org.slf4j.LoggerFactory 类的静态方法getLogger取得。 getLogger方法以 logger 名称为参数。用同一名字调用LoggerFactory.getLogger 方法所得到的永远都是同一个logger对象的引用
   * 有效级别及级别的继承
   
         Logger 可以被分配级别。级别包括：TRACE、DEBUG、INFO、WARN 和 ERROR，定义于 ch.qos.logback.classic.Level类。如果 logger没有被分配级别，那么它将从有被分配级别的最近的祖先那里继承级别。root logger 默认级别是 DEBUG。
         
   * 打印方法与基本的选择规则
         
         打印方法决定记录请求的级别。例如，如果 L 是一个 logger 实例，那么，语句 L.info("..")是一条级别为 INFO 的记录语句。记录请求的级别在高于或等于其 logger 的有效级别时被称为被启用，否则，称为被禁用。记录请求级别为 p，其logger的有效级别为 q，只有则当 p>=q时，该请求才会被执行。
         该规则是 logback 的核心。级别排序为： TRACE < DEBUG < INFO < WARN < ERROR。
 ###demo
   * 添加依赖
        
         <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
         </dependency>
         
   * 配置logback.xml
   
        <?xml version="1.0" encoding="UTF-8"?>
        <!--
        -scan:当此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true
        -scanPeriod:设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。
        -           当scan为true时，此属性生效。默认的时间间隔为1分钟
        -debug:当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。
        -
        - configuration 子节点为 appender、logger、root
        -->
        <configuration scan="true" scanPeriod="60 second" debug="false">
         
            <!-- 负责写日志,控制台日志 -->
            <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
         
                <!-- 一是把日志信息转换成字节数组,二是把字节数组写入到输出流 -->
                <encoder>
                    <Pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%5level] [%thread] %logger{0} %msg%n</Pattern>
                    <charset>UTF-8</charset>
                </encoder>
            </appender>
         
            <!-- 文件日志 -->
            <appender name="DEBUG" class="ch.qos.logback.core.FileAppender">
                <file>debug.log</file>
                <!-- append: true,日志被追加到文件结尾; false,清空现存文件;默认是true -->
                <append>true</append>
                <filter class="ch.qos.logback.classic.filter.LevelFilter">
                    <!-- LevelFilter: 级别过滤器，根据日志级别进行过滤 -->
                    <level>DEBUG</level>
                    <onMatch>ACCEPT</onMatch>
                    <onMismatch>DENY</onMismatch>
                </filter>
                <encoder>
                    <Pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%5level] [%thread] %logger{0} %msg%n</Pattern>
                    <charset>UTF-8</charset>
                </encoder>
            </appender>
         
            <!-- 滚动记录文件，先将日志记录到指定文件，当符合某个条件时，将日志记录到其他文件 -->
            <appender name="INFO" class="ch.qos.logback.core.rolling.RollingFileAppender">
                <File>info.log</File>
         
                <!-- ThresholdFilter:临界值过滤器，过滤掉 TRACE 和 DEBUG 级别的日志 -->
                <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
                    <level>INFO</level>
                </filter>
         
                <encoder>
                    <Pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%5level] [%thread] %logger{0} %msg%n</Pattern>
                    <charset>UTF-8</charset>
                </encoder>
         
                <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                    <!-- 每天生成一个日志文件，保存30天的日志文件
                    - 如果隔一段时间没有输出日志，前面过期的日志不会被删除，只有再重新打印日志的时候，会触发删除过期日志的操作。
                    -->
                    <fileNamePattern>info.%d{yyyy-MM-dd}.log</fileNamePattern>
                    <maxHistory>30</maxHistory>
                    <TimeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                        <maxFileSize>100MB</maxFileSize>
                    </TimeBasedFileNamingAndTriggeringPolicy>
                </rollingPolicy>
            </appender >
         
            <!--<!– 异常日志输出 –>-->
            <!--<appender name="EXCEPTION" class="ch.qos.logback.core.rolling.RollingFileAppender">-->
                <!--<file>exception.log</file>-->
                <!--<!– 求值过滤器，评估、鉴别日志是否符合指定条件. 需要额外的两个JAR包，commons-compiler.jar和janino.jar –>-->
                <!--<filter class="ch.qos.logback.core.filter.EvaluatorFilter">-->
                    <!--<!– 默认为 ch.qos.logback.classic.boolex.JaninoEventEvaluator –>-->
                    <!--<evaluator>-->
                        <!--<!– 过滤掉所有日志消息中不包含"Exception"字符串的日志 –>-->
                        <!--<expression>return message.contains("Exception");</expression>-->
                    <!--</evaluator>-->
                    <!--<OnMatch>ACCEPT</OnMatch>-->
                    <!--<OnMismatch>DENY</OnMismatch>-->
                <!--</filter>-->
         
                <!--<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">-->
                    <!--<!– 触发节点，按固定文件大小生成，超过5M，生成新的日志文件 –>-->
                    <!--<maxFileSize>5MB</maxFileSize>-->
                <!--</triggeringPolicy>-->
            <!--</appender>-->
         
            <appender name="ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
                <file>error.log</file>
         
                <encoder>
                    <Pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%5level] [%thread] %logger{0} %msg%n</Pattern>
                    <charset>UTF-8</charset>
                </encoder>
         
                <!-- 按照固定窗口模式生成日志文件，当文件大于20MB时，生成新的日志文件。
                -    窗口大小是1到3，当保存了3个归档文件后，将覆盖最早的日志。
                -    可以指定文件压缩选项
                -->
                <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
                    <fileNamePattern>error.%d{yyyy-MM}(%i).log.zip</fileNamePattern>
                    <minIndex>1</minIndex>
                    <maxIndex>3</maxIndex>
                    <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                        <maxFileSize>100MB</maxFileSize>
                    </timeBasedFileNamingAndTriggeringPolicy>
                    <maxHistory>30</maxHistory>
                </rollingPolicy>
            </appender>
         
            <!-- 异步输出 -->
            <appender name ="ASYNC" class= "ch.qos.logback.classic.AsyncAppender">
                <!-- 不丢失日志.默认的,如果队列的80%已满,则会丢弃TRACT、DEBUG、INFO级别的日志 -->
                <discardingThreshold >0</discardingThreshold>
                <!-- 更改默认的队列的深度,该值会影响性能.默认值为256 -->
                <queueSize>512</queueSize>
                <!-- 添加附加的appender,最多只能添加一个 -->
                <appender-ref ref ="ERROR"/>
            </appender>
         
            <!--
            - 1.name：包名或类名，用来指定受此logger约束的某一个包或者具体的某一个类
            - 2.未设置打印级别，所以继承他的上级<root>的日志级别“DEBUG”
            - 3.未设置additivity，默认为true，将此logger的打印信息向上级传递；
            - 4.未设置appender，此logger本身不打印任何信息，级别为“DEBUG”及大于“DEBUG”的日志信息传递给root，
            -  root接到下级传递的信息，交给已经配置好的名为“STDOUT”的appender处理，“STDOUT”appender将信息打印到控制台；
            -->
            <logger name="ch.qos.logback" />
         
            <!--
            - 1.将级别为“INFO”及大于“INFO”的日志信息交给此logger指定的名为“STDOUT”的appender处理，在控制台中打出日志，
            -   不再向次logger的上级 <logger name="logback"/> 传递打印信息
            - 2.level：设置打印级别（TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF），还有一个特殊值INHERITED或者同义词NULL，代表强制执行上级的级别。
            -        如果未设置此属性，那么当前logger将会继承上级的级别。
            - 3.additivity：为false，表示此logger的打印信息不再向上级传递,如果设置为true，会打印两次
            - 4.appender-ref：指定了名字为"STDOUT"的appender。
            -->
            <logger name="com.weizhi.common.LogMain" level="INFO" additivity="false">
                <appender-ref ref="STDOUT"/>
                <!--<appender-ref ref="DEBUG"/>-->
                <!--<appender-ref ref="EXCEPTION"/>-->
                <!--<appender-ref ref="INFO"/>-->
                <!--<appender-ref ref="ERROR"/>-->
                <appender-ref ref="ASYNC"/>
            </logger>
         
            <!--
            - 根logger
            - level:设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF，不能设置为INHERITED或者同义词NULL。
            -       默认是DEBUG。
            -appender-ref:可以包含零个或多个<appender-ref>元素，标识这个appender将会添加到这个logger
            -->
            <root level="DEBUG">
                <appender-ref ref="STDOUT"/>
                <!--<appender-ref ref="DEBUG"/>-->
                <!--<appender-ref ref="EXCEPTION"/>-->
                <!--<appender-ref ref="INFO"/>-->
                <appender-ref ref="ASYNC"/>
            </root>
        </configuration>
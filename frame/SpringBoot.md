##Springboot注解

**@SpringBootApplication**
        
       一个复合注解，包括@ComponentScan、@SpringBootConfiguration、@EnableAutoConfiguration。
    
**@SpringBootConfiguration**
         
       继承与@Configuration，二者功能一致，标注当前类为配置类，并且会将当前类中有@Bean注解的方法实例纳入spring容器中，实例名就是方法名。
         
**@EnableAutoConfiguration**
        
        启动自动配置
        
**@ComponentScan**
      
       扫描当前包包括子包被@Component、@Controller、@Service、@Repository注解的类纳入spring容器中。
       
       

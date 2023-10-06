能自动生成接口文档，测试接口。用于开发过程。
knife4j框架是为Java MVC框架集成Swagger生成Api文档的增强解决方案。
1. 导入knife4j 的maven坐标，在pom.xml中添加依赖
```xml
<dependency>
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>knife4j-spring-boot-starter</artifactId>
</dependency>
```
2. 在配置类中加入 knife4j 相关配置，WebMvcConfiguration.java
```java
    /**
        * 通过knife4j生成接口文档
        * @return
    */
    @Bean
    public Docket docket() {
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("苍穹外卖项目接口文档")
                .version("2.0")
                .description("苍穹外卖项目接口文档")
                .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }
```
3. 设置静态资源映射，否则接口文档页面无法访问
```java
    /**
     * 设置静态资源映射
     * @param registry
    */
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
      registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-         INF/resources/");
      registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-       INF/resources/webjars/");
    }
```
4. 访问测试：接口文档访问路径为 [http://ip:port/doc.html](http://ip:port/doc.html) ---> [http://localhost:8080/doc.html](http://localhost:8080/doc.html)
**常用注解**
`@Api`用在类上，例如Controller，表示对类的说明。（tags）
`@ApiModel`用在类上，例如entity、DTO、VO。（description）
`@ApiModelProperty`用在属性上，描述属性信息。
`@ApiOperation`用在方法上，例如Controller的方法，说明方法的用途、作用。
---
author: lexiao
comments: true
date: 2019-11-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: SpringBoot
wordpress_id: 440
tags: recent fe
categories:
- springboot
- java
- 后端
---

# 常用命令

./mvnw spring-boot:run        
                            build，run APP

mvn dependency:tree
                            打印整个依赖树



# 常用操作

## 更改 APP 启动端口

    - 在文件  【spring5webapp/】src/main/resources/application.properties
    - 添加   server.port=xxx  端口号


# 常用注解（Annotation）

## @RestController 

- 声明**类**作为Controller，类中的每个函数返回值都会转换为 HTTP RESPONSE。
- 整合了  **@Controller** and **@ResponseBody** 

```java

package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController                     //////////////
public class GreetingController {

  private static final String template = "Hello, %s!";
  private final AtomicLong counter = new AtomicLong();

  @RequestMapping("/greeting")      //////////////////
  ////////////////// defaultValue is used when "name" is missing
  public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {                 /////////////////////////
    return new Greeting(counter.incrementAndGet(),
              String.format(template, name));       ///////////////////
  }
}

```

## @RequestMapping 

- 按照 HTTP 路径规定 mapping 到对应函数
- 函数级别
- 默认是mapping所有HTTP操作（包括GET，POST等等），可以通过参数指定只绑定部分操作。

(代码如上)


## @RequestParam

- 把HTTP REQ 参数绑定为函数参数


## @SpringBootApplication

- **类**级别，用以指定程序入口函数
- 3种注解的集合
  1. **@Configuration** -- Tags the class as a source of bean definitions for the application context.
  2. @EnableAutoConfiguration -- 自动添加 bean
  3. @ComponentScan -- 自动在同一个包里面寻找 其它 component


# HTML 处理

## 导航跳转

```java

//// 在 controller 类里，使用“redirect” +  “路径”实现跳转
return "redirect:/results";

//// @Override addViewControllers(), 登记了一个名字为“results”的view，对应路径“/results”
//// 用了Thymeleaf ， 对应的是这个路径的文件  “src/main/resources/templates/results.html”
@Controller
public class WebController implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/results").setViewName("results");
    }
}

//// 这个return，对应的是 “src/main/resources/templates/form.html", 注意：不需要写 后缀 .html
  @GetMapping("/")
  public String showForm(PersonForm personForm) {
    return "form";
  }



```

## 表单数据检查

步骤：
- 使用一个 类 来对应表单数据，在该类中定义数据类型和检验规则
- 在controller类中，定义一个方法处理表单提交（POST）。该方法可以接受2个参数，第一个是 “表单数据object”(使用 @Valid 来把参数绑定到表单数据），第二个是 BindingResult 。如果提交时，表单数据不合法，那么 BindingResult 会 hasErrors()   。
- 前端网页中使用 Thymeleaf 来把表单数据绑定到对应 object 。

```java

//// 使用一个 类 来对应表单数据，在该类中定义数据类型和检验规则
import javax.validation.constraints.Min;
@Min(18)
  private Integer age;

//// 在controller类中，定义一个方法处理表单提交（POST）。该方法可以接受2个参数
@PostMapping("/")
  public String checkPersonInfo(@Valid PersonForm personForm, BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
      return "form";
    }
    return "redirect:/results";
  }

```












# Java对接支付宝支付

## 引入
为什么要发这篇帖子呢？原因很简单，就是因为在一个稍稍正规一点的应用上都会有支付这个环节，我们日常的在线支付如今包括支付宝，微信钱包，QQ钱包，银行卡支付等这些主流的支付方式，现在可能大部分人都会选择支付宝或微信钱包，当然现在网上有一些站点使用递四方支付方式，如码支付和易支付，虽然也是可以的使用微信和支付宝在线支付，但其手续费高的离谱，而且第四方支付平台不是很可靠，所以我们就不考虑他。今天就拿支付宝来说，为啥要用支付宝？原因也很简单，支付宝为开发者模拟了一套真实的支付环境供开发者使用，如果想上线也非常简单，只需要更换一些配置即可轻松上线。


## 支付流程
我们在开始coding之前先来理一下支付的流程，如果所示
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ae83bf2c-341e-4d3f-8f9b-fcd55ade753a.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ae83bf2c-341e-4d3f-8f9b-fcd55ade753a.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ae83bf2c-341e-4d3f-8f9b-fcd55ade753a.png)

给出一份支付宝官方的支付流程 [https://docs.open.alipay.com/194/105072/](https://docs.open.alipay.com/194/105072/)


## 准备
一部安卓手机(模拟支付环境  沙箱)
或
你开通了支付宝官方的支付接口，如当面付(真实支付环境)

### 沙箱环境
- 点击这里进入沙箱环境[https://openhome.alipay.com/platform/appDaily.htm](https://openhome.alipay.com/platform/appDaily.htm)
- 按照步骤进行操作，[https://docs.open.alipay.com/200/105311#s0](https://docs.open.alipay.com/200/105311#s0)
- 下载沙箱钱包，就是一个给开发者使用的支付宝 [https://openhome.alipay.com/platform/appDaily.htm?tab=tool](https://openhome.alipay.com/platform/appDaily.htm?tab=tool)
- 官方SDK地址 [https://docs.open.alipay.com/54/103419/](https://docs.open.alipay.com/54/103419/)
- 官方实例demo [https://docs.open.alipay.com/270/106291/](https://docs.open.alipay.com/270/106291/)

### 公钥设置
- 在沙箱界面点击秘钥的设置
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/98c4e54b-6f63-4509-a597-190f24b3238a.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/98c4e54b-6f63-4509-a597-190f24b3238a.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/98c4e54b-6f63-4509-a597-190f24b3238a.png)
- 如果没有设置过应该是如图所示的样子，点击公钥
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/8d16c9d3-104e-4bb2-9fa3-f4ae7d75d0ce.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/8d16c9d3-104e-4bb2-9fa3-f4ae7d75d0ce.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/8d16c9d3-104e-4bb2-9fa3-f4ae7d75d0ce.png)
- 点击支付宝秘钥生成器，然后进入下载页面，下载相应版本的生成器
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/2d6bd018-7647-43c2-916a-e4d0860d1411.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/2d6bd018-7647-43c2-916a-e4d0860d1411.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/2d6bd018-7647-43c2-916a-e4d0860d1411.png)
- 打开工具，做如图选择，然后点击生成
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/f82d6e3e-9d5c-42d1-bdf0-49a31a095b82.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/f82d6e3e-9d5c-42d1-bdf0-49a31a095b82.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/f82d6e3e-9d5c-42d1-bdf0-49a31a095b82.png)
- 然后将生成的公钥填写至刚刚打开的输入框中公钥的位置，官方文档 [https://docs.open.alipay.com/291/105971#LDsXr](https://docs.open.alipay.com/291/105971#LDsXr)
- 此时你点击查看秘钥应该就是如界面
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ca0e03bb-3bc9-4062-8a1f-2e964e018569.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ca0e03bb-3bc9-4062-8a1f-2e964e018569.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ca0e03bb-3bc9-4062-8a1f-2e964e018569.png)

## 开始开发
### 支付宝官方SDK
```xml
<dependency>
    <groupId>com.alipay.sdk</groupId>
    <artifactId>alipay-sdk-java</artifactId>
    <version>4.9.28.ALL</version>
</dependency>
```
### 创建项目骨架
- 新建一个maven项目，然后加入如下依赖，每一个依赖我都加了注释
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.kevinlu98</groupId>
    <artifactId>Alipay-Demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!--springboot集成的springmvc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>com.fasterxml.jackson.core</groupId>
                    <artifactId>jackson-databind</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--thymeleaf-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>

        <!--阿里巴巴fastjson-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.47</version>
        </dependency>

        <!--日志组件-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.26</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.26</version>
        </dependency>

        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>


        <!-- 支付宝官方SDK -->
        <dependency>
            <groupId>com.alipay.sdk</groupId>
            <artifactId>alipay-sdk-java</artifactId>
            <version>4.9.28.ALL</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

- 在resources目录下新建一个`application.yml`作为springboot的配置文件,内容如下
```yml
server:
  port: 8080
spring:
  thymeleaf:
    cache: false
    prefix: classpath:/templates/
    suffix: .html
```
- 在resources目录下新建log4j的配置文件`log4j.properties`,内容如下
```properties
log4j.rootLogger = debug,stdout,D,E

log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n

log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
log4j.appender.D.File = log.log
log4j.appender.D.Append = true
log4j.appender.D.Threshold = DEBUG
log4j.appender.D.layout = org.apache.log4j.PatternLayout
log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n

log4j.appender.E = org.apache.log4j.DailyRollingFileAppender
log4j.appender.E.File = error.log
log4j.appender.E.Append = true
log4j.appender.E.Threshold = ERROR
log4j.appender.E.layout = org.apache.log4j.PatternLayout
log4j.appender.E.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
```
- 在resources目录下新建`templates`目录作为前台页面的目录
- 创建启动类`Application`
```java
package cn.kevinlu98.alipay;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @Author: Kevin·Lu
 * @Email: kevinlu98@qq.com
 * @Date: 2020/2/20 10:41 上午
 * @Description:
 */
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

```



### 创建配置类
- 新建一个配置类`config.AlipayConfig`,复制官方实例中的AplipayConfig或者复制如下代码,修改配置信息为你自己的即可
- **注意：如果真正想应用支付宝官方的支付功能，只需要自己申请一个当面付，然后更换配置中的网关地址，支付宝公钥，应用私钥以及APPID就可以啦**

```java
package cn.kevinlu98.alipay.config;

import java.io.FileWriter;
import java.io.IOException;

/* *
 *类名：AlipayConfig
 *功能：基础配置类
 *详细：设置帐户有关信息及返回路径
 *修改日期：2017-04-05
 *说明：
 *以下代码只是为了方便商户测试而提供的样例代码，商户可以根据自己网站的需要，按照技术文档编写,并非一定要使用该代码。
 *该代码仅供学习和研究支付宝接口使用，只是提供一个参考。
 */

public class AlipayConfig {

//↓↓↓↓↓↓↓↓↓↓请在这里配置您的基本信息↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

    // 应用ID,您的APPID，收款账号既是您的APPID对应支付宝账号
    public static String app_id = "2016092500596036";

    // 商户私钥，您的PKCS8格式RSA2私钥
    public static String merchant_private_key = "MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCy8Qpyjm31dVxR0Y+0kXEGexPuThobstmzZAN89uKyQmt9nm1KhNlG8ORg4bFgrnHDae6WJJbvuaD0qb7ya+JWiyCdcZceVNrR4IlWSC68BsDVy549UQmfWpWxfR6iOSaOOa/Yr1Fy0qVfR3LpoFCfgy/XrXaYNHa8vICdkxJJgHaV0S7eZYo9Jx1IvjZN9sLLym8fjIGqJLPJ1WbgzkqNt7/ZCaPubgxg0OvTVyE6uhOkmBtiRQitovMwPb2dmg6XVHv1Kr3vhXMnOl9Z4AANhf/tKtshsvQJM0f3wcofQFqbha7RJpsFRUszTObOy4F/OS7z7G9OyMD6EoaGLkS/AgMBAAECggEAepmXlOFtCS39sLkqAodbrxsIjs/IJ44khjpSAX6N16CWUR0IuHPJAkft0UsQ4rLikwazRv+OwnSmiLr8bs/n5W+xSu4Wodt1iTKUJh+SlZTy7ghyRISPWTURNugI4xDRD8UKbCXCYi9cyqkDXHpQgtm5H8ZjaOkZKTrlzBCGCQDqP2ybwlJPP1SMiNg0OMl4ZstTJ4k/cogOd1UVLJsY9DtscDn9/+Gpi0qp8bA0ZnCwEqlhuk4rOUosKHrLf/pRrezCx0UWukoVGZDYzJddLDkGVvQGJxhJPC7E2I7JeLVQMwwVhm3AY72kjws2D2hATP0qnuLDdFFAnpxER6+50QKBgQDubzdw3xJV5yQ89MfZ09ZTYFSR65v+9Z7Rac9+zTN9gP+NayYFFjfXtFpfpjDdRZoSp1FBZ8iOyu/jil21crzvGltoRyp1e6D45nXYCQnGb/RFM4R24EFwWC0Sx2tcbjNK0DJYp7a7YjYzZ0KW6xn+y7w833T/H3B6WFbLHjiWrQKBgQDAH8+ln+tCvVHubNDaGvlCr5Hblxgj82Nuaf3UowxIQjrVNwRvl8cLhCdWhmtu5WUFXDq6cMzg7DCbZQXmbDaRKM7Kv5iZXoIK1CWQDanBmA+H+9Qh8W3FwWvSbFoFJ8VBGNmq3nI3xUcrVlIwFYJ3DU85L7MSakT9HrNtBlpymwKBgFxuZPGuqG8Awf2Xbvo0svtzdpVy3vCBy2WnPTcM2Y8nuOnbxctnB5Lpabd2t66v0sC0eD2AvDEO3tw4wYcbyb5vW0wbeow8tvSGctyi9FUnBWzmQc3LtdKVfDOxdx9H4T55Y2sW6THPKu/WcewLi/JIjNqUTcixKWtkX5EyUAGpAoGBAKq32bDHsKqGRhaB9PfJvjImhopE8buIW5NSda4MEC7pQxQRJkzu5nzyOm5lVXOePS0NLlZbQ4Kd/fcnRp3hDH/ibha1N6kY1J9AsfwWWADh2PMxr+dVfACche3eQAOSunHE3i46Ke4qy7nTo4Z8poiZeAtNumajrZfqPu+jFJ3/AoGAdx9oubnlQ/82p9iy7u27SYPLi07p7STJsE8dJUsfgvJxaDjkoAPPeidB40DeQhXPG7gLSFQ7wOcbD1rYApGGqyrDO+6Z9DY9msNbT60+4u5CI4RCho2PGbrwQQ0kXULaPF9lnWmsZ0PPdudUvyCz/pXjEwP41cIcdyb8UfTAjQk=";

    // 支付宝公钥,查看地址：https://openhome.alipay.com/platform/keyManage.htm 对应APPID下的支付宝公钥。
    public static String alipay_public_key = "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuUGtM5t51AyaxSAemD2ltPMGfNQ6ceymUlIJlhvg/X58SUFHW3gtp/LC/R5VI4kd1eItEW8mNIuMlrz+Yp+96lUDrcduuvkCfrTOkytDWLm4eMUaC4+kd9JuMwoJUCO5EQbY8TRRJlZe8xey3zilLySHvioGFHoSbDsP+sBxWelsR9frGcl1YCxixbKys4vkpwfEd/TwEcdLWpJzcFZ+3y+9K62MJwNxiA0+Zx87hnhTsLDthT/owCCJjHWYkC7ypqBsSKWAQXzqoH67+YNAlLmMpDlImXs3oay1AgnvIWy3dmfcRzH++0BZkPcIN1NdRu9h5qjJvzF4tu+DnjI6jwIDAQAB";

    // 服务器异步通知页面路径  需http://格式的完整路径，不能加?id=123这类自定义参数，必须外网可以正常访问
    public static String notify_url = "http://127.0.0.1:8080/alipay.trade.page.pay-JAVA-UTF-8/notify_url.jsp";

    // 页面跳转同步通知页面路径 需http://格式的完整路径，不能加?id=123这类自定义参数，必须外网可以正常访问
    public static String return_url = "http://127.0.0.1:8080/alipay.trade.page.pay-JAVA-UTF-8/return_url.jsp";

    // 签名方式
    public static String sign_type = "RSA2";

    // 字符编码格式
    public static String charset = "utf-8";

    // 支付宝网关
    public static String gatewayUrl = "https://openapi.alipaydev.com/gateway.do";

    // 日志目录
    public static String log_path = "./";


//↑↑↑↑↑↑↑↑↑↑请在这里配置您的基本信息↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑

    /**
     * 写日志，方便测试（看网站需求，也可以改成把记录存入数据库）
     * @param sWord 要写入日志里的文本内容
     */
    public static void logResult(String sWord) {
        FileWriter writer = null;
        try {
            writer = new FileWriter(log_path + "alipay_log_" + System.currentTimeMillis()+".txt");
            writer.write(sWord);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (writer != null) {
                try {
                    writer.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}


```


### 创建controller
- 新建一个controller类`controller.PayController`

```java
package cn.kevinlu98.alipay.controller;

import cn.kevinlu98.alipay.config.AlipayConfig;
import com.alipay.api.AlipayClient;
import com.alipay.api.DefaultAlipayClient;
import com.alipay.api.request.AlipayTradePagePayRequest;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.UnsupportedEncodingException;

/**
 * @Author: Kevin·Lu
 * @Email: kevinlu98@qq.com
 * @Date: 2020/2/20 10:59 上午
 * @Description:
 */
@Controller
@RequestMapping("/pay")
public class PayController {

    @RequestMapping("/index")
    public String payIndex() {
        return "index";
    }

    /**
     * 代码从alipay.trade.page.pay.jsp考过来即可，用于显示支付宝官方支付页面
     *
     * @param request
     * @param response
     * @throws Exception
     */
    @RequestMapping("/showPay")
    public void showPay(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //获得初始化的AlipayClient
        AlipayClient alipayClient = new DefaultAlipayClient(AlipayConfig.gatewayUrl, AlipayConfig.app_id, AlipayConfig.merchant_private_key, "json", AlipayConfig.charset, AlipayConfig.alipay_public_key, AlipayConfig.sign_type);

        //设置请求参数
        AlipayTradePagePayRequest alipayRequest = new AlipayTradePagePayRequest();
        alipayRequest.setReturnUrl(AlipayConfig.return_url);
        alipayRequest.setNotifyUrl(AlipayConfig.notify_url);

        //商户订单号，商户网站订单系统中唯一订单号，必填
        String out_trade_no = new String(request.getParameter("WIDout_trade_no").getBytes("ISO-8859-1"), "UTF-8");
        //付款金额，必填
        String total_amount = new String(request.getParameter("WIDtotal_amount").getBytes("ISO-8859-1"), "UTF-8");
        //订单名称，必填
        String subject = new String(request.getParameter("WIDsubject").getBytes("ISO-8859-1"), "UTF-8");
        //商品描述，可空
        String body = new String(request.getParameter("WIDbody").getBytes("ISO-8859-1"), "UTF-8");

        alipayRequest.setBizContent("{\"out_trade_no\":\"" + out_trade_no + "\","
                + "\"total_amount\":\"" + total_amount + "\","
                + "\"subject\":\"" + subject + "\","
                + "\"body\":\"" + body + "\","
                + "\"product_code\":\"FAST_INSTANT_TRADE_PAY\"}");


        //请求参数可查阅【电脑网站支付的API文档-alipay.trade.page.pay-请求参数】章节

        //构建支付宝官方支付页面
        String top = "<!DOCTYPE html PUBLIC \"-//W3C//DTD HTML 4.01 Transitional//EN\" \"http://www.w3.org/TR/html4/loose.dtd\">\n" +
                "<html>\n" +
                "<head>\n" +
                "<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\n" +
                "<title>付款</title>\n" +
                "</head>";
        String result = alipayClient.pageExecute(alipayRequest).getBody();
        String bottom = "<body>\n" +
                "</body>\n" +
                "</html>";
        //输出
        response.getWriter().println(top + result + bottom);
    }
}

```

- 在templates目录下新建一个`index.html`,将官方实例中的index.jsp复制过来或者复制如下代码
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>支付宝电脑网站支付</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        ul, ol {
            list-style: none;
        }

        body {
            font-family: "Helvetica Neue", Helvetica, Arial, "Lucida Grande",
            sans-serif;
        }

        .tab-head {
            margin-left: 120px;
            margin-bottom: 10px;
        }

        .tab-content {
            clear: left;
            display: none;
        }

        h2 {
            border-bottom: solid #02aaf1 2px;
            width: 200px;
            height: 25px;
            margin: 0;
            float: left;
            text-align: center;
            font-size: 16px;
        }

        .selected {
            color: #FFFFFF;
            background-color: #02aaf1;
        }

        .show {
            clear: left;
            display: block;
        }

        .hidden {
            display: none;
        }

        .new-btn-login-sp {
            padding: 1px;
            display: inline-block;
            width: 75%;
        }

        .new-btn-login {
            background-color: #02aaf1;
            color: #FFFFFF;
            font-weight: bold;
            border: none;
            width: 100%;
            height: 30px;
            border-radius: 5px;
            font-size: 16px;
        }

        #main {
            width: 100%;
            margin: 0 auto;
            font-size: 14px;
        }

        .red-star {
            color: #f00;
            width: 10px;
            display: inline-block;
        }

        .null-star {
            color: #fff;
        }

        .content {
            margin-top: 5px;
        }

        .content dt {
            width: 100px;
            display: inline-block;
            float: left;
            margin-left: 20px;
            color: #666;
            font-size: 13px;
            margin-top: 8px;
        }

        .content dd {
            margin-left: 120px;
            margin-bottom: 5px;
        }

        .content dd input {
            width: 85%;
            height: 28px;
            border: 0;
            -webkit-border-radius: 0;
            -webkit-appearance: none;
        }

        #foot {
            margin-top: 10px;
            position: absolute;
            bottom: 15px;
            width: 100%;
        }

        .foot-ul {
            width: 100%;
        }

        .foot-ul li {
            width: 100%;
            text-align: center;
            color: #666;
        }

        .note-help {
            color: #999999;
            font-size: 12px;
            line-height: 130%;
            margin-top: 5px;
            width: 100%;
            display: block;
        }

        #btn-dd {
            margin: 20px;
            text-align: center;
        }

        .foot-ul {
            width: 100%;
        }

        .one_line {
            display: block;
            height: 1px;
            border: 0;
            border-top: 1px solid #eeeeee;
            width: 100%;
            margin-left: 20px;
        }

        .am-header {
            display: -webkit-box;
            display: -ms-flexbox;
            display: box;
            width: 100%;
            position: relative;
            padding: 7px 0;
            -webkit-box-sizing: border-box;
            -ms-box-sizing: border-box;
            box-sizing: border-box;
            background: #1D222D;
            height: 50px;
            text-align: center;
            -webkit-box-pack: center;
            -ms-flex-pack: center;
            box-pack: center;
            -webkit-box-align: center;
            -ms-flex-align: center;
            box-align: center;
        }

        .am-header h1 {
            -webkit-box-flex: 1;
            -ms-flex: 1;
            box-flex: 1;
            line-height: 18px;
            text-align: center;
            font-size: 18px;
            font-weight: 300;
            color: #fff;
        }
    </style>
</head>
<body text=#000000 bgColor="#ffffff" leftMargin=0 topMargin=4>
<header class="am-header">
    <h1>支付宝电脑网站支付体验入口页</h1>
</header>
<div id="main">
    <div id="tabhead" class="tab-head">
        <h2 id="tab1" class="selected" name="tab">付 款</h2>
        <h2 id="tab2" name="tab">交 易 查 询</h2>
        <h2 id="tab3" name="tab">退 款</h2>
        <h2 id="tab4" name="tab">退 款 查 询</h2>
        <h2 id="tab5" name="tab">交 易 关 闭</h2>
    </div>
    <form name=alipayment action=alipay.trade.page.pay.jsp method=post
          target="_blank">
        <div id="body1" class="show" name="divcontent">
            <dl class="content">
                <dt>商户订单号 ：</dt>
                <dd>
                    <input id="WIDout_trade_no" name="WIDout_trade_no" />
                </dd>
                <hr class="one_line">
                <dt>订单名称 ：</dt>
                <dd>
                    <input id="WIDsubject" name="WIDsubject" />
                </dd>
                <hr class="one_line">
                <dt>付款金额 ：</dt>
                <dd>
                    <input id="WIDtotal_amount" name="WIDtotal_amount" />
                </dd>
                <hr class="one_line">
                <dt>商品描述：</dt>
                <dd>
                    <input id="WIDbody" name="WIDbody" />
                </dd>
                <hr class="one_line">
                <dt></dt>
                <dd id="btn-dd">
						<span class="new-btn-login-sp">
							<button class="new-btn-login" type="submit"
                                    style="text-align: center;">付 款</button>
						</span> <span class="note-help">如果您点击“付款”按钮，即表示您同意该次的执行操作。</span>
                </dd>
            </dl>
        </div>
    </form>
    <form name=tradequery action=alipay.trade.query.jsp method=post
          target="_blank">
        <div id="body2" class="tab-content" name="divcontent">
            <dl class="content">
                <dt>商户订单号 ：</dt>
                <dd>
                    <input id="WIDTQout_trade_no" name="WIDTQout_trade_no" />
                </dd>
                <hr class="one_line">
                <dt>支付宝交易号 ：</dt>
                <dd>
                    <input id="WIDTQtrade_no" name="WIDTQtrade_no" />
                </dd>
                <hr class="one_line">
                <dt></dt>
                <dd id="btn-dd">
						<span class="new-btn-login-sp">
							<button class="new-btn-login" type="submit"
                                    style="text-align: center;">交 易 查 询</button>
						</span> <span class="note-help">商户订单号与支付宝交易号二选一，如果您点击“交易查询”按钮，即表示您同意该次的执行操作。</span>
                </dd>
            </dl>
        </div>
    </form>
    <form name=traderefund action=alipay.trade.refund.jsp method=post
          target="_blank">
        <div id="body3" class="tab-content" name="divcontent">
            <dl class="content">
                <dt>商户订单号 ：</dt>
                <dd>
                    <input id="WIDTRout_trade_no" name="WIDTRout_trade_no" />
                </dd>
                <hr class="one_line">
                <dt>支付宝交易号 ：</dt>
                <dd>
                    <input id="WIDTRtrade_no" name="WIDTRtrade_no" />
                </dd>
                <hr class="one_line">
                <dt>退款金额 ：</dt>
                <dd>
                    <input id="WIDTRrefund_amount" name="WIDTRrefund_amount" />
                </dd>
                <hr class="one_line">
                <dt>退款原因 ：</dt>
                <dd>
                    <input id="WIDTRrefund_reason" name="WIDTRrefund_reason" />
                </dd>
                <hr class="one_line">
                <dt>退款请求号 ：</dt>
                <dd>
                    <input id="WIDTRout_request_no" name="WIDTRout_request_no" />
                </dd>
                <hr class="one_line">
                <dt></dt>
                <dd id="btn-dd">
						<span class="new-btn-login-sp">
							<button class="new-btn-login" type="submit"
                                    style="text-align: center;">退 款</button>
						</span> <span class="note-help">商户订单号与支付宝交易号二选一，如果您点击“退款”按钮，即表示您同意该次的执行操作。</span>
                </dd>
            </dl>
        </div>
    </form>
    <form name=traderefundquery
          action=alipay.trade.fastpay.refund.query.jsp method=post
          target="_blank">
        <div id="body4" class="tab-content" name="divcontent">
            <dl class="content">
                <dt>商户订单号 ：</dt>
                <dd>
                    <input id="WIDRQout_trade_no" name="WIDRQout_trade_no" />
                </dd>
                <hr class="one_line">
                <dt>支付宝交易号 ：</dt>
                <dd>
                    <input id="WIDRQtrade_no" name="WIDRQtrade_no" />
                </dd>
                <hr class="one_line">
                <dt>退款请求号 ：</dt>
                <dd>
                    <input id="WIDRQout_request_no" name="WIDRQout_request_no" />
                </dd>
                <hr class="one_line">
                <dt></dt>
                <dd id="btn-dd">
						<span class="new-btn-login-sp">
							<button class="new-btn-login" type="submit"
                                    style="text-align: center;">退 款 查 询</button>
						</span> <span class="note-help">商户订单号与支付宝交易号二选一，如果您点击“退款查询”按钮，即表示您同意该次的执行操作。</span>
                </dd>
            </dl>
        </div>
    </form>
    <form name=tradeclose action=alipay.trade.close.jsp method=post
          target="_blank">
        <div id="body5" class="tab-content" name="divcontent">
            <dl class="content">
                <dt>商户订单号 ：</dt>
                <dd>
                    <input id="WIDTCout_trade_no" name="WIDTCout_trade_no" />
                </dd>
                <hr class="one_line">
                <dt>支付宝交易号 ：</dt>
                <dd>
                    <input id="WIDTCtrade_no" name="WIDTCtrade_no" />
                </dd>
                <hr class="one_line">
                <dt></dt>
                <dd id="btn-dd">
						<span class="new-btn-login-sp">
							<button class="new-btn-login" type="submit"
                                    style="text-align: center;">交 易 关 闭</button>
						</span> <span class="note-help">商户订单号与支付宝交易号二选一，如果您点击“交易关闭”按钮，即表示您同意该次的执行操作。</span>
                </dd>
            </dl>
        </div>
    </form>
    <div id="foot">
        <ul class="foot-ul">
            <li>支付宝版权所有 2015-2018 ALIPAY.COM</li>
        </ul>
    </div>
</div>
</body>
<script language="javascript">
    var tabs = document.getElementsByName('tab');
    var contents = document.getElementsByName('divcontent');

    (function changeTab(tab) {
        for(var i = 0, len = tabs.length; i < len; i++) {
            tabs[i].onmouseover = showTab;
        }
    })();

    function showTab() {
        for(var i = 0, len = tabs.length; i < len; i++) {
            if(tabs[i] === this) {
                tabs[i].className = 'selected';
                contents[i].className = 'show';
            } else {
                tabs[i].className = '';
                contents[i].className = 'tab-content';
            }
        }
    }

    function GetDateNow() {
        var vNow = new Date();
        var sNow = "";
        sNow += String(vNow.getFullYear());
        sNow += String(vNow.getMonth() + 1);
        sNow += String(vNow.getDate());
        sNow += String(vNow.getHours());
        sNow += String(vNow.getMinutes());
        sNow += String(vNow.getSeconds());
        sNow += String(vNow.getMilliseconds());
        document.getElementById("WIDout_trade_no").value =  sNow;
        document.getElementById("WIDsubject").value = "测试";
        document.getElementById("WIDtotal_amount").value = "0.01";
    }
    GetDateNow();
</script>
</html>
```

### 创建回调controller
- 新建一个controller类`controller.CallbackController`

```java
package cn.kevinlu98.alipay.controller;

import cn.kevinlu98.alipay.config.AlipayConfig;
import com.alipay.api.internal.util.AlipaySignature;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import java.io.PrintWriter;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

/**
 * @Author: Kevin·Lu
 * @Email: kevinlu98@qq.com
 * @Date: 2020/2/20 11:50 上午
 * @Description: 回调
 */
@Controller
@RequestMapping("/callback")
public class CallbackController {
    /**
     * 同步回调地址 从return_url.jsp拷贝
     *
     * @param request
     * @param response
     * @throws Exception
     */
    @RequestMapping("/return")
    public void returnUrl(HttpServletRequest request, HttpServletResponse response) throws Exception {
        PrintWriter out = response.getWriter();
        //获取支付宝GET过来反馈信息
        Map<String, String> params = new HashMap<String, String>();
        Map<String, String[]> requestParams = request.getParameterMap();
        for (Iterator<String> iter = requestParams.keySet().iterator(); iter.hasNext(); ) {
            String name = (String) iter.next();
            String[] values = (String[]) requestParams.get(name);
            String valueStr = "";
            for (int i = 0; i < values.length; i++) {
                valueStr = (i == values.length - 1) ? valueStr + values[i]
                        : valueStr + values[i] + ",";
            }
            //乱码解决，这段代码在出现乱码时使用
            valueStr = new String(valueStr.getBytes("ISO-8859-1"), "utf-8");
            params.put(name, valueStr);
        }

        boolean signVerified = AlipaySignature.rsaCheckV1(params, AlipayConfig.alipay_public_key, AlipayConfig.charset, AlipayConfig.sign_type); //调用SDK验证签名

        //——请在这里编写您的程序（以下代码仅作参考）——
        if (signVerified) {
            System.out.println("同步验证成功");
            //商户订单号
            String out_trade_no = new String(request.getParameter("out_trade_no").getBytes("ISO-8859-1"), "UTF-8");

            //支付宝交易号
            String trade_no = new String(request.getParameter("trade_no").getBytes("ISO-8859-1"), "UTF-8");

            //付款金额
            String total_amount = new String(request.getParameter("total_amount").getBytes("ISO-8859-1"), "UTF-8");

            out.println("trade_no:" + trade_no + "<br/>out_trade_no:" + out_trade_no + "<br/>total_amount:" + total_amount);
        } else {
            out.println("验签失败");
        }
        //——请在这里编写您的程序（以上代码仅作参考）——
//        return "";
    }

    /**
     * 异步回调地址 从notify_url.jsp拷贝
     *
     * @param request
     * @param response
     * @throws Exception
     */
    @RequestMapping("/notify")
    public void notifyUrl(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("异步验证");
        PrintWriter out = response.getWriter();
        //获取支付宝POST过来反馈信息
        Map<String, String> params = new HashMap<String, String>();
        Map<String, String[]> requestParams = request.getParameterMap();
        for (Iterator<String> iter = requestParams.keySet().iterator(); iter.hasNext(); ) {
            String name = (String) iter.next();
            String[] values = (String[]) requestParams.get(name);
            String valueStr = "";
            for (int i = 0; i < values.length; i++) {
                valueStr = (i == values.length - 1) ? valueStr + values[i]
                        : valueStr + values[i] + ",";
            }
            //乱码解决，这段代码在出现乱码时使用
            valueStr = new String(valueStr.getBytes("ISO-8859-1"), "utf-8");
            params.put(name, valueStr);
        }

        boolean signVerified = AlipaySignature.rsaCheckV1(params, AlipayConfig.alipay_public_key, AlipayConfig.charset, AlipayConfig.sign_type); //调用SDK验证签名

        //——请在这里编写您的程序（以下代码仅作参考）——

	/* 实际验证过程建议商户务必添加以下校验：
	1、需要验证该通知数据中的out_trade_no是否为商户系统中创建的订单号，
	2、判断total_amount是否确实为该订单的实际金额（即商户订单创建时的金额），
	3、校验通知中的seller_id（或者seller_email) 是否为out_trade_no这笔单据的对应的操作方（有的时候，一个商户可能有多个seller_id/seller_email）
	4、验证app_id是否为该商户本身。
	*/
        if (signVerified) {//验证成功
            System.out.println("异步验证成功");
            //商户订单号
            String out_trade_no = new String(request.getParameter("out_trade_no").getBytes("ISO-8859-1"), "UTF-8");

            //支付宝交易号
            String trade_no = new String(request.getParameter("trade_no").getBytes("ISO-8859-1"), "UTF-8");

            //交易状态
            String trade_status = new String(request.getParameter("trade_status").getBytes("ISO-8859-1"), "UTF-8");

            if (trade_status.equals("TRADE_FINISHED")) {
                //判断该笔订单是否在商户网站中已经做过处理
                //如果没有做过处理，根据订单号（out_trade_no）在商户网站的订单系统中查到该笔订单的详细，并执行商户的业务程序
                //如果有做过处理，不执行商户的业务程序
                System.out.println("订单已完成");
                //注意：
                //退款日期超过可退款期限后（如三个月可退款），支付宝系统发送该交易状态通知
            } else if (trade_status.equals("TRADE_SUCCESS")) {
                //判断该笔订单是否在商户网站中已经做过处理
                //如果没有做过处理，根据订单号（out_trade_no）在商户网站的订单系统中查到该笔订单的详细，并执行商户的业务程序
                //如果有做过处理，不执行商户的业务程序
                //我们的业务代码
                System.out.println("订单付款成功");
                //注意：
                //付款完成后，支付宝系统发送该交易状态通知
            }

            out.println("success");

        } else {//验证失败
            out.println("fail");

            //调试用，写文本函数记录程序运行情况是否正常
            //String sWord = AlipaySignature.getSignCheckContentV1(params);
            //AlipayConfig.logResult(sWord);
        }

        //——请在这里编写您的程序（以上代码仅作参考）——
//        return "";
    }
}

```


## 效果演示
- 浏览器输入[http://127.0.0.1:8080/pay/index](http://127.0.0.1:8080/pay/index)

[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/dff610b5-5f89-4663-83ee-01317d6e6d8f.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/dff610b5-5f89-4663-83ee-01317d6e6d8f.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/dff610b5-5f89-4663-83ee-01317d6e6d8f.png)

- 点击付款

[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ee61d4ba-b6f5-4cae-ab85-281c1cc99e6f.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ee61d4ba-b6f5-4cae-ab85-281c1cc99e6f.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/ee61d4ba-b6f5-4cae-ab85-281c1cc99e6f.png)

- 手机扫码
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/1861e860-bd35-40df-8221-07b1eb2a4830.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/1861e860-bd35-40df-8221-07b1eb2a4830.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/1861e860-bd35-40df-8221-07b1eb2a4830.png)

- 付款成功

[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/64de665f-0129-4aa1-ba0d-d69a6e17a403.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/64de665f-0129-4aa1-ba0d-d69a6e17a403.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/64de665f-0129-4aa1-ba0d-d69a6e17a403.png)

- 自动跳转到我们的同步回调页面
[![https://gitee.com/kevinlu98/imgbed/raw/master/20200220/17e82c6b-b6a7-437c-b036-e8abe9014d3f.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/17e82c6b-b6a7-437c-b036-e8abe9014d3f.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200220/17e82c6b-b6a7-437c-b036-e8abe9014d3f.png)


## 源码下载
- github: [https://github.com/kevinlu98/alipay-demo](https://github.com/kevinlu98/alipay-demo)
- gitee: [https://gitee.com/kevinlu98/alipay-demo](https://gitee.com/kevinlu98/alipay-demo)
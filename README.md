# Custom-Auto-Configuration

Create custom auto-configuration in spring boot how to achieve it. And how to disable or exclude existing auto configurations? Give the example 


1. 📦 How to Create Your Own Auto-Configuration

Auto-configuration = when Spring Boot automatically configures beans based on classpath or properties.

To create your own custom auto-configuration, follow these steps:

✅ Step-by-Step:

1.1 Create your custom class (service or logic)

		package com.example.demo.service;

		public class MyService {
			public String serve() {
				return "Service called!";
			}
		}

1.2 Create your Auto-Configuration class

		package com.example.demo.autoconfig;

		import com.example.demo.service.MyService;
		import org.springframework.context.annotation.Bean;
		import org.springframework.context.annotation.Configuration;
		import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;

		@Configuration
		public class MyServiceAutoConfiguration {

			@Bean
			@ConditionalOnMissingBean
			public MyService myService() {
				return new MyService();
			}
		}

@Configuration → it's a config class.

@Bean → defines a Spring bean.

@ConditionalOnMissingBean → only creates MyService if no user-defined MyService bean exists.

1.3 Register the Auto-Configuration
👉 Create a file:


		src/main/resources/META-INF/spring.factories
		
Add:


		org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
		com.example.demo.autoconfig.MyServiceAutoConfiguration

✅ Now, Spring Boot will automatically pick your MyServiceAutoConfiguration at startup!

2. 🛑 How to Disable or Exclude Auto-Configurations
Spring Boot gives you multiple ways to disable unwanted auto-configurations.

✅ 2.1 Using exclude attribute in @SpringBootApplication

		import org.springframework.boot.autoconfigure.SpringBootApplication;
		import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;

		@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
		public class DemoApplication {
			public static void main(String[] args) {
				SpringApplication.run(DemoApplication.class, args);
			}
		}
		
Here we exclude DataSourceAutoConfiguration.

Good when you don't want certain default beans to be created (like DataSource, Jpa, etc.).

✅ 2.2 Using application.properties or application.yml

spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
Or in YAML:


		spring:
		  autoconfigure:
			exclude:
			  - org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
			  
3. ✨ Final Quick Summary

Task	How to Do
Create Custom Auto-Config	1) Write a @Configuration class, 2) Add it to spring.factories
Disable Auto-Config	1) @SpringBootApplication(exclude = {}), 2) spring.autoconfigure.exclude in properties

Use @ConditionalOnClass, @ConditionalOnProperty, etc. for even smarter auto-configuration.

Example:

		@Bean
		@ConditionalOnClass(name = "com.example.SomeClass")
		public MyService myService() {
			return new MyService();
		}
So the bean will only be created if SomeClass is on the classpath!









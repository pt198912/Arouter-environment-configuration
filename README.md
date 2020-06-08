# Arouter-environment-configuration
1、项目根目录添加

     buildscript {
        ext.arouter_register_version = '1.0.2'
     }
     classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
     classpath "com.alibaba:arouter-register:$arouter_register_version"
     
2、在公共依赖库common-lib的build.gradle中添加

    apply plugin: 'kotlin-android'
    apply plugin: 'kotlin-android-extensions'
    apply plugin: 'kotlin-kapt'
    kapt {
      arguments {
          arg("AROUTER_MODULE_NAME", project.getName())
      }
    }
    注意与dependencies{
    
    }同级
    在dependencies中添加 
    //ARouter
    compile 'com.alibaba:arouter-api:1.2.2'
    kapt 'com.alibaba:arouter-compiler:1.2.2'
    
3、在其他需要调用arouter的module的build.gradle中添加

    apply plugin: 'kotlin-android-extensions'
    apply plugin: 'kotlin-android'
    apply plugin: 'com.alibaba.arouter'
    
    android{
      defaultConfig{
          javaCompileOptions{
             annotationProcessorOptions{
                 arguments = [AROUTER_MODULE_NAME: project.getName(), AROUTER_GENERATE_DOC: "enable"]
             }
          }
      }
    }
    dependencies {
      //arouter
      api 'com.alibaba:arouter-api:1.2.2'
      annotationProcessor 'com.alibaba:arouter-compiler:1.2.2'
    }

4.在application中初始化

    if (isDebugARouter) {
         ARouter.openLog();
         ARouter.openDebug();
         ARouter.printStackTrace();
    }
 
    ARouter.init(this);
    
5、声明

    @Route(path="/main/stamp_activity")
    
6、调用

    // 1. Simple jump within application (Jump via URL in 'Advanced usage')
    ARouter.getInstance().build("/test/activity").navigation();

    // 2. Jump with parameters
    ARouter.getInstance().build("/test/1")
                .withLong("key1", 666L)
                .withString("key3", "888")
                .withObject("key4", new Test("Jack", "Rose"))
                .navigation();

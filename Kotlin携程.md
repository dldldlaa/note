## Kotlin携程

并发：在应用中执行多项任务

协程：挂起是指挂起代码，稍后执行。借助协程，可以执行并发

### 同步代码

协程（co-routine)  代码挂起等待时，其他代码可以执行

### 挂起函数



挂起点是指函数内可挂起函数执行的位置，恢复后，代码会从挂起的地方执行其他函数

```kotlin
import kotlinx.coroutines.*
import kotlin.system.*
fun main() {
    
    val time =measureTimeMillis {
          runBlocking{
            println("Weather forecast")
            
            printForecast()
            printTemperature()
            
    }
       
        
    }
       println("花费时间${time/1000}")  //2s
    
  
}
    
    suspend fun printForecast(){
        delay(1000)
    println("Sunny")
    }
    
    suspend fun printTemperature(){
        delay(1000)
        println("hsah")
    }


```

### 异步代码

launch()

```kotlin
import kotlinx.coroutines.*
import kotlin.system.*
fun main() {
    
    val time =measureTimeMillis {
          runBlocking{
            println("Weather forecast")
            
            launch{
                        printForecast()
            }
                launch{
                       printTemperature()
            }


            
    }
       
        
    }
       println("花费时间${time/1000}")  //2s
    
  
}
    
    suspend fun printForecast(){
        delay(1000)
    println("Sunny")
    }
    
    suspend fun printTemperature(){
        delay(1000)
        println("hsah")
    }


```

### async()

```kotlin
import kotlinx.coroutines.*
import kotlin.system.*
fun main() {
    
    val time =measureTimeMillis {
          runBlocking{
            println("Weather forecast")
              
             println(getWeatherReport())
            
              println("Have a good day!")    
    }
      
        
    }
         
       println("花费时间${time/1000}")
    
  
}
    
    suspend fun getForecast() :String{
        delay(1000)
        
        return "Sunny"
  
    }
    
    suspend fun getTemperature():String{
        delay(1000)
      
        return "hsah"
    }

//新建一个挂起函数
suspend fun getWeatherReport()=CoroutineScope{
    val forecast=async{getForecast()}
    val temperature=async{getTemperature()}
    
      println("${forecast.await()} ${temperature.await()}")  //记住加上await()
            
}

```

养成一些习惯~  

### 异常和取消

协程：协程之间存在父子关系

自学是需要时刻思考的！不能欺骗自己！多思考！勤加练习！那里没看懂，多看几遍！看的效果太差！必须练习！

```kotlin
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        println(getWeatherReport())
        println("Have a good day!")
    }
}

suspend fun getWeatherReport() = coroutineScope {
    val forecast = async { getForecast() }
    val temperature = async { getTemperature() }
    "${forecast.await()} ${temperature.await()}"
}

suspend fun getForecast(): String {
    delay(1000)
    return "Sunny"
}

suspend fun getTemperature(): String {
    delay(1000)
    throw AssertionError("the weather is invalid") //这里添加异常提示
    return "30\u00b0C"
}


//报错之后的代码不会再执行
Weather forecast
Exception in thread "main" java.lang.AssertionError: the weather is invalid
```

### try catch 捕获

```kotlin
fun main() {
    runBlocking {
        println("Weather forecast")
         try{
            //可能出错的代码
                println(getWeatherReport())
        }catch(e:AssertionError){
                   println("Caught exception in runBlocking(): $e")
        }
        println("Have a good day!")
    }
}
//现在可以正常输出
Weather forecast
Caught exception in runBlocking(): java.lang.AssertionError: the weather is invalid
Have a good day!
```

升级：提升用户体验

```
suspend fun getWeatherReport() = coroutineScope {
    val forecast = async { getForecast() }
    val temperature = async {
    
      try{
            //可能出错的代码
             getTemperature()
        }catch(e:AssertionError){
                   println("Caught exception in runBlocking(): $e")
        }
    
    }
    "${forecast.await()} ${temperature.await()}"
}
```

之后聊天！不需要多聊！什么事情都是需要自己理性分析的！



### 取消

转：https://medium.com/androiddevelopers/coroutines-first-things-first-e6187bf3bb21

## 协程：要事第一

本系列博客文章深入探讨了协程中的取消和异常。取消很重要，可以避免做更多的工作，会更加浪费记忆和生活的能量。合适的异常处理是用户更加体验的关键。作为其他系列的基础，定义一些核心协程概念，比如：CoroutineScope，Job和CoroutineContext很重要，因此我们的目标一致。

### CoroutineScope

CoroutineScope可以持续跟踪你使用launch或async创建的任何协程（这些是CoroutineScope的延申函数）。正在进行的工作可以调用Scope.cancel()取消。

你应该创建一个CoroutineScope在你想开始和控制

## 支付订单事件埋点说明

针对支付订单事件，强关联属性（如支付金额、支付方式、所属银行、是否成功）需要在 HOW 中携带。其中“是否成功”必须等待前端拿到后端处理结果后再上报，才能准确反映支付结果。如果关注的是用户的支付意愿，则在用户点击“支付订单”按钮时即可进行前端采集，这样既能捕捉意图，又不会依赖后端回执。

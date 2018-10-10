## Java：单例模式的七种写法

---
##### 第一种（懒汉，线程不安全）：

```
public class Singleton {  
      private static Singleton instance;  
      private Singleton (){
      }   
      public static Singleton getInstance() {  
      if (instance == null) {  
          instance = new Singleton();  
      }  
      return instance;  
      }  
 }  
```
懒汉模式申明了一个静态对象，在用户第一次调用时初始化，虽然节约了资源，但第一次加载时需要实例化，反映稍慢一些，而且在多线程不能正常工作。
##### 第二种（饿汉）：

```
public class Singleton {  
     private static Singleton instance = new Singleton();  
     private Singleton (){
     }
     public static Singleton getInstance() {  
     return instance;  
     }  
 }  
```
避免了多线程的同步问题，不过，instance在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用getInstance方法， 但是也不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化instance显然没有达到lazy loading的效果。
##### 第三种（懒汉，线程安全）：

```
public class Singleton {  
      private static Singleton instance;  
      private Singleton (){
      }
      public static synchronized Singleton getInstance() {  
      if (instance == null) {  
          instance = new Singleton();  
      }  
      return instance;  
      }  
 }  
```
同步开销大，不建议。

##### 第四种（Double Check Locking）：

```
public class Singleton {  
      private volatile static Singleton singleton;  
      private Singleton (){
      }   
      public static Singleton getInstance() {  
      if (instance== null) {  
          synchronized (Singleton.class) {  
          if (instance== null) {  
              instance= new Singleton();  
          }  
         }  
     }  
     return singleton;  
     }  
 }

```
DCL模式在java1.5之前由于java内存模型而无法真正生效，1.5之后才加入volatile关键字使得DCL起作用。具体的解析看一看下面这篇文章，虽然多方资料表示该方法可行，但是并不推荐，一是代码偏繁琐，而是有更好的解决方案。
https://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html

##### 第五种（静态内部类）：

```
public class Singleton { 
    private Singleton(){
    }
      public static Singleton getInstance(){  
        return SingletonHolder.sInstance;  
    }  
    private static class SingletonHolder {  
        private static final Singleton sInstance = new Singleton();  
    }  
} 

```
第一次加载Singleton类时并不会初始化sInstance，只有第一次调用getInstance方法时虚拟机加载SingletonHolder 并初始化sInstance ，这样不仅能确保线程安全也能保证Singleton类的唯一性

##### 第六种（枚举）：

```
public enum Singleton {  
     INSTANCE;  
     public void doSomeThing() {  
     }  
 } 
 
public enum DataSourceEnum {
    DATASOURCE;
    private DBConnection connection = null;

    private DataSourceEnum() {
        connection = new DBConnection();
    }

    public DBConnection getConnection() {
        return connection;
    }
}  
```
枚举单例的优点就是功能完整、使用简洁、无偿地提供了序列化机制、在面对复杂的序列化或者反射攻击时仍然可以绝对防止多次实例化，但是枚举是新特性，大部分应用开发很少用枚举，可读性并不是很高。

##### 第七种（使用容器实现单例模式）：

```
public class SingletonManager { 
　　private static Map<String, Object> objMap = new HashMap<String,Object>();
　　private Singleton() { 
　　}
　　public static void registerService(String key, Objectinstance) {
　　　　if (!objMap.containsKey(key) ) {
　　　　　　objMap.put(key, instance) ;
　　　　}
　　}
　　public static ObjectgetService(String key) {
　　　　return objMap.get(key) ;
　　}
}
```
用SingletonManager 将多种的单例类统一管理，在使用时根据key获取对象对应类型的对象。这种方式使得我们可以管理多种类型的单例，并且在使用时可以通过统一的接口进行获取操作，降低了用户的使用成本，也对用户隐藏了具体实现，降低了耦合度。

### 枚举实现单例原理
##### 为什么枚举会满足线程安全、序列化等标准。

在JDK5 中提供了大量的语法糖，枚举就是其中一种。
所谓 语法糖（Syntactic Sugar），也称糖衣语法，是由英国计算机学家 Peter.J.Landin
 发明的一个术语，指在计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是但是更方便程序员使用。只是在编译器上做了手脚，却没有提供对应的指令集来处理它。

就拿枚举来说，其实Enum就是一个普通的类，它继承自java.lang.Enum类。

```
public enum DataSourceEnum {

    DATASOURCE;

}  
```
把上面枚举编译后的字节码反编译，得到的代码如下：

```
public final class DataSourceEnum extends Enum<DataSourceEnum> {
      public static final DataSourceEnum DATASOURCE;
      public static DataSourceEnum[] values();
      public static DataSourceEnum valueOf(String s);
      static {};
}
```

由反编译后的代码可知，DATASOURCE 被声明为 static 的，虚拟机会保证一个类的<clinit>() 方法在多线程环境中被正确的加锁、同步。所以，枚举实现是在实例化时是线程安全。
      
Java规范中规定，每一个枚举类型极其定义的枚举变量在JVM中都是唯一的，因此在枚举类型的序列化和反序列化上，Java做了特殊的规定。
在序列化的时候Java仅仅是将枚举对象的name属性输出到结果中，反序列化的时候则是通过 java.lang.Enum 的 valueOf() 方法来根据名字查找枚举对象。
也就是说，序列化的时候只将 DATASOURCE 这个名称输出，反序列化的时候再通过这个名称，查找对于的枚举类型，因此反序列化后的实例也会和之前被序列化的对象实例相同。




## Java：序列化

---


### 什么是序列化
以特定的方式对类实例的瞬时状态进行编码保存的一种操作，叫做对象序列化。就是将对象的这个时刻的各种属性各种值按照一定的规则变成二进制流，然后如果传输到别的jvm中，jvm可以按照规则在将二进制流反序列化成对应的对象，并且对象里面还有当时的数据和各种属性

### 序列化的指标
1. 对象序列化后的大小
一个对象会被序列化工具序列化为一串byte数组，这其中包含了对象的field值以及元数据信息，使其可以被反序列化回一个对象
2. 序列化与反序列化的速度
一个对象被序列化成byte数组的时间取决于它生成/解析byte数组的方法
3. 序列化工具本身的速度
序列化工具本身创建会有一定的消耗。

### 各种序列化的对比
##### java序列化
原理：类需要实现 Serializable接口，才能被jdk自己的序列化机制序列化，jdk序列化的时候，会将这个类和他的所有超类都元数据，类描述，属性，属性值等等信息都序列化出来，这样就导致序列化后的大小比较大，速度也会比较慢，但是包含的内容最全面。可以完全反序列化。

###### 缺点
1. 无法跨语言。这应该是java序列化最致命的问题了。由于java序列化是java内部私有的协议，其他语言不支持，导致别的语言无法反序列化，这严重阻碍了它的应用。
2. 序列后的码流太大。java序列化的大小是二进制编码的5倍多！
3. 序列化性能太低。java序列化的性能只有二进制编码的6.17倍，可见java序列化性能实在太差了。

```
public final void writeObject(Object x) throws IOException
public final Object readObject() throws IOException, 
                                 ClassNotFoundException
```
##### Kryo序列化

原理：序列化的时候，会将对象的信息，对象属性值的信息等进行序列化，而且没有将类field的描述信息进行序列化，这样就比jdk自己的序列化出来的小多了，而且速度肯定更快，但是包含的信息没有jdk的全面。
```
   public static void main(String[] args) throws IOException {
        Kryo kryo = new Kryo();

        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        Output output = new Output(baos);

        Map<String, Object> map = new HashMap<String, Object>();
        map.put("a", 1);
        map.put("b", 2);

        kryo.writeObject(output, map);
        output.close();

        baos.flush();

        byte[] bytes = baos.toByteArray();

        Input input = new Input(new ByteArrayInputStream(bytes));
        Map<String, Object> newmap = kryo.readObject(input, HashMap.class);
        input.close();

        System.out.println(newmap);
    }
```

##### Fst
FST fast-serialization 是重新实现的 Java 快速对象序列化的开发包。序列化速度更快（2-10倍）、体积更小，而且兼容 JDK 原生的序列化。要求 JDK 1.7 支持。

```
    public static void main(String[] args) {
        FSTConfiguration fst = FSTConfiguration.createDefaultConfiguration();

        Map<String, Object> map = new HashMap<String, Object>();
        map.put("a", 1);
        map.put("b", 2);

        byte[] bytes = fst.asByteArray(map);
        System.out.println(bytes.length);

        Map<String, Object> newmap = (Map) fst.asObject(bytes);

        System.out.println(newmap);
    }
```

##### Protostuff
protobuf是谷歌推出的与语言无关、平台无关的通信协议，一个对象经过protobuf序列化后将变成二进制格式的数据，所以他可读性差，但换来的是占用空间小，速度快。使用protobuf要先使用特定的语法编写一个.proto文件，该文件与语言无关，然后使用特殊的编译器对该文件进行编译，生成与语言相关的文件
   
protostuff不需要依赖.proto文件，他可以直接对普通的javabean进行序列化、反序列化的操作，而效率上甚至比protobuf还快

```
    public static class Bean {
        private String a;
        private int b;

        public String getA() {
            return a;
        }

        public int getB() {
            return b;
        }

        public void setA(String a) {
            this.a = a;
        }

        public void setB(int b) {
            this.b = b;
        }
    }

    public static <T> byte[] serializer(T o) {
        Schema schema = RuntimeSchema.getSchema(o.getClass());
        return ProtobufIOUtil.toByteArray(o, schema, LinkedBuffer.allocate(256));
    }

    public static <T> T deserializer(byte[] bytes, Class<T> clazz) {

        T obj = null;
        try {
            obj = clazz.newInstance();
            Schema schema = RuntimeSchema.getSchema(obj.getClass());
            ProtostuffIOUtil.mergeFrom(bytes, obj, schema);
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }

        return obj;
    }

    public static void main(String[] args) {
        Bean bean = new Bean();
        bean.setA("1");
        bean.setB(2);

        byte[] bytes = serializer(bean);
        System.out.println(bytes.length);

        Bean newmap = deserializer(bytes, Bean.class);

        System.out.println(newmap);
    }
```




框架   | 优点  |  缺点
---|---|---
Kryo | 速度快，序列化后体积小 | 跨语言支持较复杂
Protostuff |速度快，基于protobuf |需静态编译
Protostuff-Runtime | 无需静态编译，但序列化前需预先传入schema | 不支持无默认构造函数的类，反序列化时需用户自己初始化序列化后的对象，其只负责将该对象进行赋值
Java|使用方便，可序列化所有类 |速度慢，占空间




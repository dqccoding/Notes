[TOC]

### 1.重要概念：

   > 1.RabbitMq Server：也叫broker server。任务就是维护一条从producer到consumer的路线，保证数据能够按照指定的防止进行传输。
   > 2.Producer：消息生产者，消息的发送方，将消息发送到Exchange
   > 3.Consumer：消息消费者，RabbitMq会将Queue中的消息传递给消费者
   > 4.Exchange：交换器，有direct，fanout，topic，headers四种类型，每种拥有不同的路由规则，本身不存储数据。
   > 5.Queue：队列，在rmq内部负责存储消息。消息消费者通过订阅队列来获取消息，Rmq中的消息都只能存储在Queue中，生产者生产消息并最终投递到Queue中，然后消费者再去利用。多对多关系。
   > 6.RoutingKey：路由规则，通过指定路由规则指定消息流向哪里。

### 2. Exchange Types(交换器类型)

   RabbitMQ 常用的 Exchange Type 有 **fanout**、**direct**、**topic**、**headers** 这四种（AMQP规范里还提到两种 Exchange Type，分别为 system 与 自定义，这里不予以描述）。、

**性能排序：fanout > direct >> topic。比例大约为11：10：6**

 ##### ① fanout

   fanout 类型的Exchange路由规则非常简单，它会把所有发送到该Exchange的消息路由到所有与它绑定的Queue中，不需要做任何判断操作，所以 fanout 类型是所有的交换机类型里面速度最快的。fanout 类型常用来广播消息。

##### ② direct

   direct 类型的Exchange路由规则也很简单，它会把消息路由到那些 Bindingkey 与 RoutingKey 完全匹配的 Queue 中。

   ![direct 类型交换器](D:\learn\Notes\Mq\RabbitMq.assets\687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d31322d31362f33373030383032312e6a7067)

   以上图为例，如果发送消息的时候设置路由键为“warning”,那么消息会路由到 Queue1 和 Queue2。如果在发送消息的时候设置路由键为"Info”或者"debug”，消息只会路由到Queue2。如果以其他的路由键发送消息，则消息不会路由到这两个队列中。

   direct 类型常用在处理有优先级的任务，根据任务的优先级把消息发送到对应的队列，这样可以指派更多的资源去处理高优先级的队列。
##### ③ topic

   前面讲到direct类型的交换器路由规则是完全匹配 BindingKey 和 RoutingKey ，但是这种严格的匹配方式在很多情况下不能满足实际业务的需求。topic类型的交换器在匹配规则上进行了扩展，它与 direct 类型的交换器相似，也是将消息路由到 BindingKey 和 RoutingKey 相匹配的队列中，但这里的匹配规则有些不同，它约定：

   - RoutingKey 为一个点号“．”分隔的字符串（被点号“．”分隔开的每一段独立的字符串称为一个单词），如 “com.rabbitmq.client”、“java.util.concurrent”、“com.hidden.client”;

   - BindingKey 和 RoutingKey 一样也是点号“．”分隔的字符串；

   - BindingKey 中可以存在两种特殊字符串“*”和“#”，用于做模糊匹配，其中“\*”用于匹配一个单词，“#”用于匹配多个单词(可以是零个)。![topic 类型交换器](D:\learn\Notes\Mq\RabbitMq.assets\687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d31322d31362f37333834332e6a7067)

     以上图为例：

     - 路由键为 “com.rabbitmq.client” 的消息会同时路由到 Queuel 和 Queue2;
     - 路由键为 “com.hidden.client” 的消息只会路由到 Queue2 中；
     - 路由键为 “com.hidden.demo” 的消息只会路由到 Queue2 中；
     - 路由键为 “java.rabbitmq.demo” 的消息只会路由到Queuel中；
     - 路由键为 “java.util.concurrent” 的消息将会被丢弃或者返回给生产者（需要设置 mandatory 参数），因为它没有匹配任何路由键。
##### ④ headers(不推荐)

headers 类型的交换器不依赖于路由键的匹配规则来路由消息，而是根据发送的消息内容中的 headers 属性进行匹配。在绑定队列和交换器时制定一组键值对，当发送消息到交换器时，RabbitMQ会获取到该消息的 headers（也是一个键值对的形式)'对比其中的键值对是否完全匹配队列和交换器绑定时指定的键值对，如果完全匹配则消息会路由到该队列，否则不会路由到该队列。headers 类型的交换器性能会很差，而且也不实用，基本上不会看到它的存在。

### 3. 

### 4. 

### 5.参考文章

1. [RabbitMQ消息确认、消息持久化等核心知识总结](https://www.jianshu.com/p/cc3d2017e7b3)
2. 

   


















# ✉️ 消息

一个 "消息元素`MessageElement`" 仅能代表一个消息元素，通常一条消息中常含有多种元素混合，例如文字与图片的混合。因此有了 "消息链`MessageChain`" 来表示混合有多种消息元素的一条消息。

消息链本质是一个消息组件的有序集合，在大多数情况下，消息链是只读的。

{% hint style="info" %}
例外：Image 在被发送的时会检查其图片是否已被上传，而这一步必须修改 Image 本身的属性，导致 Image 破坏了只读特性。
{% endhint %}

## 消息元素类型

* Source 消息的 id
* Quote 引用其他消息
* Plain 纯文本
* Face 自带表情
* Poke 戳一戳
* At @
* AtAll @全体成员
* Image 图片
* Flash 闪照
* Video 小视频
* Voice 语音
* Json 卡片
* Xml 卡片
* App 小程序
* Node 引用的消息段
* Music 音乐分享

{% hint style="info" %}
Image, Flash, Video, Voice 都涉及 IO 操作，使用时需要特殊关注。
{% endhint %}

{% hint style="warning" %}
Json, Xml, App 通常不可用，就算可用，那也是用来把帐号搞进风险控制用的。
{% endhint %}

## 消息构造

有多种方法可以创造一个消息链：

#### 使用 `MessageChainBuilder`

```csharp
var chain = new MessageChainBuilder()
    .AddAtAll()
    .AddPlain("爸爸们好!")
    .Build();
```

#### 使用构造函数

```csharp
var chain = new MessageChain(new MessageElement[] { new Plain("我是伞兵") });
```

#### 使用 `MessageChain.Construct` 方法

```csharp
var chain = MessageChain.Construct(new Plain("你才是伞兵"));
```


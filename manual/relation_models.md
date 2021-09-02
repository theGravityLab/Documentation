# 关系
群，成员，好友都是一种关系和关系的组合，在 Hyperai 中，使用 "关系模型`RelationModel`"  来表示存在这样的关系，也仅只表示关系，不含对应的任何操作，也不应该含有操作。以下列举了各种代表这些关系的特定类型。

### 特定关系

##### 自己(Self)
自己表示一个自己，提供关于这个账户有关的信息，例如好友列表和加入得到群的列表。

##### 好友(Friend)
好友表示一个好友，提供关于这个好友的一些属性。

##### 群(Group)
群表示一个群，提供关于这个群的一些属性和相关的群成员列表。

##### 成员(Member)
成员表示一个成员，提供关于这个成员的一些属性和其所在的群。

### 关系模型的性质

每个 `RelationModel` 都有一个 `Identity:long` 属性和 `Identifier:string` 属性。前者在 OICQ 中表示 QQ 号或者群号，后者则被 Hyperai 用来区分同一个用户在不同情况（好友或群成员）的独特身份。在不同的关系模型中 `Identifier` 具有以下参考：

```csharp
Self.Identifier => this.Identity.ToString();
Friend.Identifier => this.Identity.ToString();
Group.Identifier => this.Identity.ToString();
Member.Identifier => $"{this.Group.Identity}@{this.Identity}";
```

除了 `Member` 其他的类型 `Identifier` 都等于其 `Identity`， 而 `Member` 的 `Identifier` 则由自己的和群的 `Identity` 构成。这意味着当 `Member` 类型脱离了 `Group` 之后将无法被正常使用。只有成员在群被定义的时候其本身才是有意义的。

### 使用 `Signature` 匹配关系
群和成员好友都可以使用它们的 `Identifier` 来标识一个实例，但该方法并不方便，`Signature` 除了能匹配单个的用户外，还可以用于筛选一个群里的所有成员，或是某个用户的群关系和好友关系。

`Signature` 使用一个如 `{group}:{user}` 的字符串形来匹配，其规则如下：

group
- \* : 所有的群和好友中
- \_: 不在群内，即在还有中
- 数字: 由该数字表示的特定群内

user
- \*: 任意一个用户
- 数字: 由该数字表示的特定用户
# osgFramework
>缩写osgFW
>
本工程的主要目的是为了整理一下近10年来在行为化场景管理上的一些总结。最初设计这套框架的灵感来自于director和flash。后来经过了多个版本的重构,删除的代码已不计其数，光是硬盘里的版本分支和修改已经超过了20个。自己都迷糊了：<

本项目的核心功能是对场景的管理。先说说OSG有什么不方便吧。
* 1 osg的Node结构是多子节点多父节点的，空间关系上无法用简单的方式表达。虽然有NodePath这个类型，但是他无法转换成任何可记录的数据形式。Node的name属性是没有任何限制的，也导致了通过名称搜索根本无法保证得到有效的结果。所以大家习惯性的把node指针保存的到处都是。
* 2 osg是通过对Node的继承来实现具体功能的，这个做法滥用了继承关系。而c++又是一个include关系的管理模式。这就很容易导致大量的循环引用。
* 3 osg里的消息处理对象满天飞。想给一个node写update更新都要给自己dynamic_cast一下。
* 4 通讯机制完全没有。正如第一条说的，只能依靠各种node的指针来通讯。太乱。
* 5 osg自带的GUI设计完全跟场景设计是分离的。
* 6 osg也没有独立的2D对象支持，也没有布局管理。


本项目的设计理念
* 1 使用SceneNode来管理场景树，并且强制同级SceneNode不存在重名的情况，这样可以使用一个类似 "earth|tree|apple" 的字符串来表达NodePath。这样场景关系非常单纯，从scene对象开始每个名字路径都有唯一的SceneNode对应。
* 2 通过给每个SceneNode增加 behavior 的方式来定义不同的行为模式。对行为的继承不会导致include混乱。
* 3 实现3D对象和2D对象的统一管理。（GUI是通过2D对象+行为的模式实现的）
* 4 支持对2D对象的布局管理。


代码上设计了三组具体对象
* SceneNode 场景空间关系管理
* Behavior  对象行为管理
* Shape     实际渲染对象 （作为SceneNode的唯一的子对象）

为了保证SceneNode不能继承自任何Node，这里使用了给node的userdata里写入结构体的方式来让node升级为SceneNode。这种模式配合FnSceneNode类型的操作，使用了bridge设计模式。

同样对Behavior的操作也是对应的BehBehavior<T>，只是这里复杂了一些。继承一个新的Behavior需要做三件事：
* 继承 BehaviorOption （这里你可以理解为初始化参数表）
* 继承 Behavior
* typedef BehBehavior<Behavior2DSprite,Behavior2DSpriteOption> BehSprite2D; （就是这样）


待续。。。

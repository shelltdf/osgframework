# osgframework
本工程的主要目的是为了整理一下近10来在行为化场景管理上的一些总结。最初设计这套框架的灵感来自于director和flash。后来经过了多个版本的重构，光是硬盘里的版本分支和修改已经超过了20个。自己都迷糊了：<

本项目的核心功能是对场景的管理
* 1 使用SceneNode来管理场景树，并且强制同级SceneNode不存在重名的情况，这样可以使用一个类似 "earth|tree|apple" 的字符串来表达NodePath。
* 2 通过给每个SceneNode增加 behavior 的方式来定义不同的行为模式。
* 3 实现场景中3D对象和2D对象的统一管理。（GUI是通过2D对象+行为的模式实现的）


设计上体现了三组概念
* SceneNode 场景空间关系管理
* Behavior  对象行为管理
* Shape     实际渲染对象 （作为SceneNode的唯一的子对象）

为了保证SceneNode不能继承自任何Node，这里使用了给node的userdata里写入结构体的方式来让node升级为SceneNode。这种模式配合FnSceneNode类型的操作，使用了bridge设计模式。

同样对Behavior的操作也是对应的BehBehavior<T>，只是这里复杂了一些。继承一个新的Behavior需要做三件事：
* 继承 BehaviorOption （这里你可以理解为初始化参数表）
* 继承 Behavior
* typedef BehBehavior<Behavior2DSprite,Behavior2DSpriteOption> BehSprite2D; （就是这样）


待续。。。

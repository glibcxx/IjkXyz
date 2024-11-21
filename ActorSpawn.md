title: 实体创建过程

## 1. 构造实体

构造实体使用工厂模式，游戏通过 ActorFactory 类负责所有 Actor 对象的构造。每种实体都有自己的构造入口，大部分都是直接调用构造函数创建一个智能指针。
工厂内查找并调用目标实体的构造入口，得到对象的 unique 指针，然后设置坐标和旋转角，如果构造函数内没有设置包围盒(AABB)，那么会先默认使用玩家 AABB，

最后初始化实体组件部分，注册到实体系统中。

实体系统(EntitySystem)是 ECS 架构(Entity-Component-System, 实体-组件-系统)的系统部分，基岩版采用的实现为 EnTT 库。
实体系统中的实体只是一个标识符，为明确概念，我将实体系统中的实体称为 Entity，其他的也就是游戏中通常意义上的实体称作 Actor。

注册 Entity 的类为 EntityRegistry。

此时 Actor 应当包含一个 ActorComponent，用于在实体系统中找到一个 Entity 对应的 Actor。

弹射物为例，弹射物一般由 Spawner 类创建。该类先通过实体工厂获得弹射物对象，再确定弹射物的全局属性，然后添加到世界中，最后赋予弹射物初速度将其射出。

## 2. 添加实体

添加实体由 Level 的 addEntity(或 addGlobalEntity)函数完成，其赋予 runtimeId 后将实体添加到所在区块的实体列表中，然后调用 Actor 的 reload 函数，进行实体初始化。

## 3. 初始化实体

初始化实体由 Actor 的 reload 函数完成，先添加剩余的实体组件，不同实体有不同的组件。有些组件仅控制属性，比如 physicsComponent；有些组件需要在实体系统更新阶段专门处理，弹射物组件。

处理完组件后，再处理硬编码数据，由 Acotr 的 reloadHardcoded 函数完成，比如有些实体没有在构造函数内设置 AABB，那么会在这个函数内设置。

最后实体创建完成，将 Initialized 标记为 true。

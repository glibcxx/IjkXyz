---
title: 物品
---

## class Item

```cpp
class Item;
```

Item 类，所有物品类的基类。属性包括 id、名称、是否是实验性内容、掉落物是否会消失等。

mLegacyBlock 属性：如果是方块此属性有效，否则为空。

useOn 成员函数：被使用时调用，具体使用逻辑在 \_useOn 函数

## class BlockItem

```cpp
class BlockItem : public Item
```

所有方块的物品都是 BlockItem 类的对象(待验证)。没有额外属性。

\_useOn 成员函数：包含高度检测，放置方块的逻辑。

## class ItemStackBase

```cpp
class ItemStackBase;
```

抽象类，定义游戏中实际的一个或数个堆叠的物品的基本属性和行为。

包含 Item 的弱指针，代表内部物品。

mBlock 属性，方块，与 mLegacyBlock 重复(？).

其他属性：数据值，已堆叠数量等。

## class ItemStack

```cpp
class ItemStack : public ItemStackBase
```

ItemStackBase 的一个实现类，不清楚有什么特别的功能。

## class ItemInstance

```cpp
class ItemInstance : public ItemStackBase
```

ItemStackBase 的一个实现类，不清楚有什么特别的功能。

ItemStack 与 ItemInstance 的对象可以互相构造。

## 其他

### 方块变物品

方块被破坏，

```cpp
BlockLegacy::playerDestroy(...)
 -> BlockLegacy::spawnResources(...)
 -> BlockLegacy::popResource(...)
 -> Spawner::spawnItem(...)
```

spawnResources 还会调用 getResourceItem 获取方块的物品，调用 getResourceCount 获取数量，调用 ExperienceOrb::spawnOrbs(...)生成经验球

### 物品使用

```cpp
???(...)
 -> ItemUseInventoryTransaction::handle(...)
 -> PlayerInventoryProxy::createTransactionContext(...)
 -> Container::createTransactionContext()
 -> ItemUseInventoryTransaction::handle(Player &,bool)const::$_2::operator()
 -> GameMode::baseUseItem(...)
 -> PlayerInventoryProxy::createTransactionContext(...)
 -> Container::createTransactionContext()
 -> GameMode::baseUseItem(ItemStack &)::$_9::operator() (callback)
 -> GameMode::useItem(...)
 -> ItemStack::use(...)
 -> Item::use(...)
```

Item 子类重写的 Item::use(...)函数实现物品被使用后的效果，比如雪球被射出。

### 掉落物

掉落物是物品实体的表现形式

```cpp
class ItemActor : public Actor
```

ItemActor 类包含一个 ItemStack 对象，和掉落时间等属性。

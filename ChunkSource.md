---
title: ChunkSource
---

---

-   Level
    -   Dimension
        -   ChunkSource (MainChunkSource)
            -   LevelChunk
        -   WorldGenerator
        -   BlockSource
-   Player
    -   ChunkViewSource
        -   ChunkSourceView `GridArea<std::shared_ptr<LevelChunk>>`
            -   LevelChunk
        -   ChunkSource `mMainChunkSource`
    -   BlockSource

---

-   Level-Dimension: 维度
-   ChunkSource: 区块源，区块的数据源于此，对于维度，其实现为生成器 (WorldGenerator)
    -   OverworldGenerator
    -   NetherGenerator
    -   TheEndGenerator
    -   ChunkSource 的实例间可存在父子关系
-   xxxGenerator: 继承自 ChunkSource 和 WorldGenerator，每个为维度有一个对象作为区块源
-   WorldGenerator: 生成器接口类
-   LevelChunk: 区块
-   MainChunkSource: 继承自 ChunkSource，是一个封装类，内部就是上面的三大生成器
    -   MainChunkSource 是生成区块的入口，内部调用三大生成器的生成函数，还包含一个非持有区块列表
    -   Dimension 持有的 ChunkSource 为 MainChunkSource
-   Player-ChunkViewSource: 继承自 ChunkSource，是一个封装类，内部包含一个区块视图(ChunkSourceView)，表示由这个玩家加载的区块。内部对象为 MainChunkSource。
    -   ChunkSourceView: 区块视图，是 GridArea 的实现，内部包含一个区域，随玩家移动
        -   LevelChunk: 区块视图内，玩家加载的区块列表
    -   ChunkSource: 为 MainChunkSource 对象，来自 Dimension
    -   区块视图移动时，通过 ChunkSource 创建新加入的 LevelChunk 对象，存入区块视图的区块列表中，移出的区块则被保存
    -   区块数据由 ChunkSourceView 持有，MainChunkSource 不持有数据，后者通过弱指针获取前者拥有的区块。
-   BlockSource: 又称 region，由玩家和 Dimension 各自独立持有，该类用来表示一个区域，通过这个类可以获取区域内的数据和信息，内包含一个 ChunkSource 的引用
    Player-BlockSource: ChunkSource 引用玩家的 ChunkSourceView
    Dimension-BlockSource: ChunkSource 引用维度的 MainChunkSource

---

-   region 获取方块：
    1. 获取 region 内的 ChunkSource
    2. 查找 ChunkSource 内的区块
    3. 从区块查找子区块
    4. 从子区块获取方块
-   区块视图的区块列表只包含玩家自己加载的区块，MainChunkSource 通过弱指针获取被玩家等加载的区块。
-   区块视图的范围由视距确定，与模拟距离无关
-   玩家的区块视图是一个圆形（待定）
-   Dimension 内的 MainChunkSource 对象的父对象为 DBChunkStorage，父对象的父对象为 xxxGenerator。DBChunkStorage 是一个从存档中读取数据的区块源，这样的父子关系是为了先试图从存档中读取区块，没有再新生成区块

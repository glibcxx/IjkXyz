## Type definition

```cpp
class DefaultEntitySystemsCollection {
public:
    struct ECSTiming {
        int   mCount{0};
        float mMsTime{0};
    };
    struct TickingSystemsInCategory {
        Bedrock::typeid_t<struct SystemCategory> mCategory;
        std::vector<uint>                        mSystems{};
        std::vector<ECSTiming>                   mTimings{};
    };

    std::vector<std::shared_ptr<ISystem>> mAllSystems;
    std::vector<InternalSystemInfo>       mAllSystemsInfo;
    uint64_t                              mUnk;
    std::vector<TickingSystemsInCategory> mTickingSystemCategories;
    std::vector<TickingSystemsInCategory> mSingleTickingSystemCategories;
    entt::dense_map<uint, ComponentInfo>  mAllComponentsInfo;
    std::mutex                            mTimingMutex;
};
```

## vtable

```
_anonymous_namespace_::DefaultEntitySystemsCollection::_scalar_deleting_destructor_
_anonymous_namespace_::DefaultEntitySystemsCollection::registerTickingSystem
_anonymous_namespace_::DefaultEntitySystemsCollection::registerSystem
_anonymous_namespace_::DefaultEntitySystemsCollection::foreachSystem
_anonymous_namespace_::DefaultEntitySystemsCollection::foreachTickingSystem
_anonymous_namespace_::DefaultEntitySystemsCollection::foreachSingleTickingSystem
_anonymous_namespace_::DefaultEntitySystemsCollection::getSystemInfoForTickingSystemId
_anonymous_namespace_::DefaultEntitySystemsCollection::getTickingSystemForTickingSystemId
_anonymous_namespace_::DefaultEntitySystemsCollection::getComponentInfoForId
_anonymous_namespace_::DefaultEntitySystemsCollection::foreachComponentInfo
_anonymous_namespace_::DefaultEntitySystemsCollection::hasSingleTickCategory
_anonymous_namespace_::DefaultEntitySystemsCollection::gatherSystemTimings
```

## System registry

```cpp
void DefaultEntitySystemsCollection::registerTickingSystem(
    gsl::span<const Bedrock::typeid_t<struct SystemCategory>> cates,
    std::unique_ptr<ITickingSystem>                           system,
    const SystemInfo&                                         sysInfo,
    EntitySystemTickingMode                                   mode
) {
    this->internalRegisterSystem(std::move(system), sysInfo, [this, &cates, &mode](uint id) {
        for (auto&& iter : cates) {
            if (mode.mNormalTick) {
                bool found = false;
                for (auto&& jter : this->mTickingSystemCategories) {
                    if (jter.mCategory != iter) continue;
                    found = true;
                    break;
                }
                if (!found) {
                    this->mTickingSystemCategories.emplace_back(TickingSystemsInCategory{iter});
                }
            }
            if (mode.mSingleTick) {
                bool found = false;
                for (auto&& jter : this->mSingleTickingSystemCategories) {
                    if (jter.mCategory != iter) continue;
                    found = true;
                    break;
                }
                if (!found) {
                    auto iter2 = this->mSingleTickingSystemCategories.emplace_back(TickingSystemsInCategory{iter});
                    iter2.mSystems.emplace_back(id);
                }
            }
        }
    });
}

template <typename Fn>
void DefaultEntitySystemsCollection::internalRegisterSystem(
    std::shared_ptr<ISystem> system,
    const SystemInfo&        sysInfo,
    Fn&&                     callback
) {
    uint size = this->mAllSystems.size();
    this->mAllSystems.emplace_back(system);
    if (sysInfo.mGenerateDetailedInfo) {
        std::vector<ComponentInfo> infos = ((std::vector<ComponentInfo>(*)())(sysInfo.mGenerateDetailedInfo))();
        for (auto&& info : infos) {
            this->mAllComponentsInfo.emplace(info.mId, std::move(info));
        }
    }
    this->mAllSystemsInfo.emplace_back(InternalSystemInfo{sysInfo});
    callback(size);
}
```
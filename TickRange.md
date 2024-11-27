---
title: tick range
---

```cpp
void tickingRange(const int serverTickRange = 8)
{
    uint8_t   list[serverTickRange * 3 + 1][serverTickRange * 3 + 1]{};
    for (int vertical = 0; vertical < serverTickRange; ++vertical)
    {
        for (int horizontal = -vertical - 1; horizontal <= vertical + 1; ++horizontal)
        {
            ++list[(int)(serverTickRange * 1.5f) + horizontal][(int)(serverTickRange * 1.5f) + serverTickRange - vertical];
            ++list[(int)(serverTickRange * 1.5f) + horizontal][(int)(serverTickRange * 1.5f) + vertical - serverTickRange];
            if (vertical == serverTickRange - 1)
            {
                ++list[(int)(serverTickRange * 1.5f) + horizontal][(int)(serverTickRange * 1.5f) + 0];
            }
        }
    }
    for (int i = 0; i < serverTickRange * 3 + 1; i++)
    {
        for (int j = 0; j < serverTickRange * 3 + 1; j++)
        {
            std::print("{} ", list[i][j] ? "▣" : "▢");
        }
        std::println("");
    }
}
```

```
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▣ ▣ ▣ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢ ▢
```

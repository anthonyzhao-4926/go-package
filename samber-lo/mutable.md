# samber/lo - Mutable 可变包文档（lo/mutable）

## 概述

Mutable 包（`lo/mutable`）提供原地修改集合的工具函数，适用于性能关键型场景。与核心包（`lo`）的不可变操作不同，这些函数直接修改输入切片或映射，避免额外的内存分配。

> 源文档：https://lo.samber.dev/docs/mutable
> 官方仓库：https://github.com/samber/lo
> 包导入：`github.com/samber/lo/mutable`

---

## Slice（切片相关）

### Filter

原地过滤切片，仅保留使谓词返回 true 的元素。直接修改输入切片。

```go
import lom "github.com/samber/lo/mutable"

list := []int{1, 2, 3, 4}
kept := lom.Filter(list, func(x int) bool {
    return x%2 == 0
})
// list 现在是 []int{2, 4}
// kept 指向修改后的同一底层数组

func Filter[T any, Slice ~[]T](collection Slice, predicate func(item T) bool) Slice
```

### Map

原地转换切片中的每个元素。直接修改输入切片。

```go
list := []int{1, 2, 3, 4}
lom.Map(list, func(x int) int {
    return x * 2
})
// list 现在是 []int{2, 4, 6, 8}

func Map[T any, Slice ~[]T](collection Slice, transform func(item T) T)
```

### Reverse

原地反转切片，使首元素变为末元素。

```go
list := []int{0, 1, 2, 3, 4, 5}
lom.Reverse(list)
// list 现在是 []int{5, 4, 3, 2, 1, 0}

func Reverse[T any, Slice ~[]T](collection Slice)
```

### Shuffle

使用 Fisher–Yates 算法原地随机打乱切片。

```go
list := []int{0, 1, 2, 3, 4, 5}
lom.Shuffle(list)
// list 顺序被随机打乱，例如 []int{1, 4, 0, 3, 5, 2}

func Shuffle[T any, Slice ~[]T](collection Slice)
```

### Fill

用指定的初始值填充切片的所有元素。

```go
slice := make([]int, 5)
lom.Fill(slice, 42)
// slice 现在是 []int{42, 42, 42, 42, 42}

func Fill[T any, Slice ~[]T](collection Slice, initial T)
```

### Compact

原地移除切片中的零值。

```go
list := []int{0, 1, 2, 0, 3, 0}
kept := lom.Compact(list)
// list 现在是 []int{1, 2, 3}
// kept 指向修改后的同一底层数组

func Compact[T comparable, Slice ~[]T](collection Slice) Slice
```

---

## 性能考量

**何时使用 Mutable**

- 处理大型集合时内存分配成为瓶颈
- 在紧循环中重复过滤/转换相同的切片
- 不需要保留原始数据

**何时避免 Mutable**

- 需要函数式编程风格（不修改输入）
- 切片作为函数参数被多处共享引用
- 需要并发安全

**内存行为**

所有 Mutable 函数都在原地操作，返回修改后的切片。底层数组不会被重新分配，仅元素被移动或修改。这意味着：

```go
original := []int{1, 2, 3, 4, 5}
result := lom.Filter(original, func(x int) bool {
    return x > 2
})
// result 是 []int{3, 4, 5}
// original 也指向同一修改后的底层数组的一部分
// 两者现在都是同一底层数组的视图
```

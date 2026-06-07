# samber/lo - Go 泛型工具库：核心模块文档

## 概述

本文档是 samber/lo 核心包（`lo`）的完整参考，包含 300+ 个不可变工具函数，涵盖切片、映射、通道、字符串、数学、并发、错误处理等常见操作。所有函数均基于 Go 1.18+ 泛型实现，类型安全。

> 源文档：https://lo.samber.dev/docs/core
> 官方仓库：https://github.com/samber/lo

---

## 切片操作

### Count

统计集合中等于给定值的元素数量。

```go
lo.Count([]int{1, 5, 1}, 1) // 2

func Count[T comparable](collection []T, value T) int
```

### CountBy

统计集合中使谓词函数返回 true 的元素数量。

```go
lo.CountBy([]int{1, 5, 1}, func(i int) bool {
    return i < 4
}) // 2

func CountBy[T any](collection []T, predicate func(item T) bool) int
```

### CountValues

统计集合中每个元素出现的次数。

```go
lo.CountValues([]int{1, 2, 2})
// map[int]int{1: 1, 2: 2}

func CountValues[T comparable](collection []T) map[T]int
```

### CountValuesBy

统计每个转换后值出现的次数（等价于先 Map 再 CountValues）。

```go
isEven := func(v int) bool { return v%2 == 0 }
lo.CountValuesBy([]int{1, 2, 2}, isEven)
// map[bool]int{false: 1, true: 2}

func CountValuesBy[T any, U comparable](collection []T, transform func(item T) U) map[U]int
```

### Filter

遍历集合，返回所有使谓词函数返回 `true` 的元素组成的切片。

```go
even := lo.Filter([]int{1, 2, 3, 4}, func(x int, index int) bool {
    return x%2 == 0
}) // []int{2, 4}

func Filter[T any, Slice ~[]T](collection Slice, predicate func(item T, index int) bool) Slice
```

### Map

使用函数将切片中的每个元素转换为新类型。

```go
transformed := lo.Map([]int64{1, 2, 3, 4}, func(x int64, index int) string {
    return strconv.FormatInt(x, 10)
})
// []string{"1", "2", "3", "4"}

func Map[T any, R any](collection []T, transform func(item T, index int) R) []R
```

### FlatMap

转换切片并将其展平为另一类型的切片。

```go
out := lo.FlatMap([]int64{0, 1, 2}, func(x int64, _ int) []string {
    return []string{strconv.FormatInt(x, 10), strconv.FormatInt(x, 10)}
})
// []string{"0", "0", "1", "1", "2", "2"}

func FlatMap[T any, R any](collection []T, transform func(item T, index int) []R) []R
```

### FilterMap

通过给定回调函数同时执行过滤和映射，返回结果切片。

```go
matching := lo.FilterMap([]string{"cpu", "gpu", "mouse", "keyboard"},
    func(x string, _ int) (string, bool) {
        if strings.HasSuffix(x, "pu") {
            return "xpu", true
        }
        return "", false
    })
// []string{"xpu", "xpu"}

func FilterMap[T any, R any](collection []T, callback func(item T, index int) (R, bool)) []R
```

### Reduce

通过累加器函数将集合归约为单个值。

```go
sum := lo.Reduce([]int{1, 2, 3, 4}, func(agg int, item int, _ int) int {
    return agg + item
}, 0) // 10

func Reduce[T any, R any](collection []T, accumulator func(agg R, item T, index int) R, initial R) R
```

### ForEach

遍历集合中的元素，并对每个元素调用回调函数。

```go
lo.ForEach([]string{"hello", "world"}, func(x string, _ int) {
    println(x)
})

func ForEach[T any](collection []T, callback func(item T, index int))
```

### Uniq

返回去重后的切片副本，仅保留每个值的首次出现。

```go
lo.Uniq([]int{1, 2, 2, 1}) // []int{1, 2}

func Uniq[T comparable, Slice ~[]T](collection Slice) Slice
```

### GroupBy

根据从每个元素计算出的键对元素进行分组。

```go
groups := lo.GroupBy([]int{0, 1, 2, 3, 4, 5}, func(i int) int {
    return i % 3
})
// map[int][]int{0: {0, 3}, 1: {1, 4}, 2: {2, 5}}

func GroupBy[T any, U comparable, Slice ~[]T](collection Slice, iteratee func(item T) U) map[U]Slice
```

### Chunk

将切片按给定大小拆分为多个块。

```go
lo.Chunk([]int{0, 1, 2, 3, 4, 5}, 2)
// [][]int{{0, 1}, {2, 3}, {4, 5}}

func Chunk[T any, Slice ~[]T](collection Slice, size int) []Slice
```

### Drop

从切片开头丢弃 n 个元素。

```go
lo.Drop([]int{0, 1, 2, 3, 4, 5}, 2) // []int{2, 3, 4, 5}

func Drop[T any, Slice ~[]T](collection Slice, n int) Slice
```

### Take

从切片开头取出前 n 个元素。

```go
lo.Take([]int{0, 1, 2, 3, 4, 5}, 3) // []int{0, 1, 2}

func Take[T any, Slice ~[]T](collection Slice, n int) Slice
```

### Reverse

原地反转切片并返回它。已弃用：请使用 `mutable.Reverse`。

```go
lo.Reverse([]int{0, 1, 2, 3, 4, 5}) // []int{5, 4, 3, 2, 1, 0}

func Reverse[T any, Slice ~[]T](collection Slice) Slice
```

### Concat

返回包含所有集合中全部元素的新切片。

```go
list1 := []int{0, 1}
list2 := []int{2, 3, 4, 5}
combined := lo.Concat(list1, list2) // []int{0, 1, 2, 3, 4, 5}

func Concat[T any, Slice ~[]T](collections ...Slice) Slice
```

### Flatten

将切片的切片展平一层。

```go
flat := lo.Flatten([][]int{{0, 1}, {2, 3, 4, 5}})
// []int{0, 1, 2, 3, 4, 5}

func Flatten[T any, Slice ~[]T](collection []Slice) Slice
```

### Repeat

构建一个包含 N 个初始值副本的切片。

```go
lo.Repeat(5, "42")
// []string{"42", "42", "42", "42", "42"}

func Repeat[T any](count int, initial T) []T
```

### KeyBy

使用回调函数计算键，将切片转换为映射。

```go
m := lo.KeyBy([]string{"a", "aa", "aaa"}, func(str string) int {
    return len(str)
})
// map[int]string{1: "a", 2: "aa", 3: "aaa"}

func KeyBy[K comparable, V any](collection []V, iteratee func(item V) K) map[K]V
```

### Reject

返回使谓词函数返回 false 的元素（与 Filter 相反）。

```go
lo.Reject([]int{1, 2, 3, 4}, func(x int, _ int) bool {
    return x%2 == 0
}) // []int{1, 3}

func Reject[T any, Slice ~[]T](collection Slice, predicate func(item T, index int) bool) Slice
```

### Compact

返回所有非零值元素组成的切片。

```go
lo.Compact([]string{"", "foo", "", "bar", ""})
// []string{"foo", "bar"}

func Compact[T comparable, Slice ~[]T](collection Slice) Slice
```

---

## 映射操作

### Keys

提取一个或多个映射的键到切片中。

```go
keys := lo.Keys(map[string]int{"foo": 1, "bar": 2})
// []string{"foo", "bar"}

func Keys[K comparable, V any](in ...map[K]V) []K
```

### UniqKeys

跨多个映射生成去重后的键切片。

```go
keys := lo.UniqKeys(map[string]int{"foo": 1, "bar": 2}, map[string]int{"bar": 3})
// []string{"foo", "bar"}

func UniqKeys[K comparable, V any](in ...map[K]V) []K
```

### HasKey

判断映射中是否存在指定的键。

```go
exists := lo.HasKey(map[string]int{"foo": 1}, "foo") // true

func HasKey[K comparable, V any](in map[K]V, key K) bool
```

### Values

提取一个或多个映射的值到切片中。

```go
values := lo.Values(map[string]int{"foo": 1, "bar": 2}) // []int{1, 2}

func Values[K comparable, V any](in ...map[K]V) []V
```

### UniqValues

跨一个或多个映射提取去重后的值切片。

```go
values := lo.UniqValues(map[string]int{"foo": 1, "bar": 2}, map[string]int{"bar": 2})
// []int{1, 2}

func UniqValues[K comparable, V comparable](in ...map[K]V) []V
```

### ValueOr

返回指定键对应的值，若键不存在则返回回退值。

```go
value := lo.ValueOr(map[string]int{"foo": 1}, "baz", 42) // 42

func ValueOr[K comparable, V any](in map[K]V, key K, fallback V) V
```

### PickBy

返回根据键/值谓词过滤后的同类型映射。

```go
m := lo.PickBy(map[string]int{"foo": 1, "bar": 2, "baz": 3},
    func(key string, value int) bool { return value%2 == 1 })
// map[string]int{"foo": 1, "baz": 3}

func PickBy[K comparable, V any, Map ~map[K]V](in Map, predicate func(key K, value V) bool) Map
```

### PickByKeys

返回仅保留指定键的同类型映射。

```go
m := lo.PickByKeys(map[string]int{"foo": 1, "bar": 2, "baz": 3}, []string{"foo", "baz"})
// map[string]int{"foo": 1, "baz": 3}

func PickByKeys[K comparable, V any, Map ~map[K]V](in Map, keys []K) Map
```

### PickByValues

返回仅保留值在给定值列表中的条目（同类型映射）。

```go
m := lo.PickByValues(map[string]int{"foo": 1, "bar": 2, "baz": 3}, []int{1, 3})
// map[string]int{"foo": 1, "baz": 3}

func PickByValues[K comparable, V comparable, Map ~map[K]V](in Map, values []V) Map
```

### OmitBy

返回排除掉匹配谓词的条目后的同类型映射。

```go
m := lo.OmitBy(map[string]int{"foo": 1, "bar": 2, "baz": 3},
    func(key string, value int) bool { return value%2 == 1 })
// map[string]int{"bar": 2}

func OmitBy[K comparable, V any, Map ~map[K]V](in Map, predicate func(key K, value V) bool) Map
```

### OmitByKeys

返回移除指定键后的映射（PickByKeys 的反向操作）。

```go
m := lo.OmitByKeys(map[string]int{"foo": 1, "bar": 2, "baz": 3}, []string{"foo", "baz"})
// map[string]int{"bar": 2}

func OmitByKeys[K comparable, V any, Map ~map[K]V](in Map, keys []K) Map
```

### OmitByValues

返回移除值在给定值列表中的条目后的映射（PickByValues 的反向操作）。

```go
m := lo.OmitByValues(map[string]int{"foo": 1, "bar": 2, "baz": 3}, []int{1, 3})
// map[string]int{"bar": 2}

func OmitByValues[K comparable, V comparable, Map ~map[K]V](in Map, values []V) Map
```

### Entries / ToPairs

将映射转换为键值对（Entry）切片。`ToPairs` 是它的别名。

```go
entries := lo.Entries(map[string]int{"foo": 1, "bar": 2})
// []lo.Entry[string, int]{
//     {Key: "foo", Value: 1},
//     {Key: "bar", Value: 2},
// }

func Entries[K comparable, V any](in map[K]V) []Entry[K, V]
func ToPairs[K comparable, V any](in map[K]V) []Entry[K, V] // 别名
```

### FromEntries / FromPairs

从键值对（Entry）切片重建映射。`FromPairs` 是它的别名。

```go
m := lo.FromEntries([]lo.Entry[string, int]{
    {Key: "foo", Value: 1},
    {Key: "bar", Value: 2},
})
// map[string]int{"foo": 1, "bar": 2}

func FromEntries[K comparable, V any](entries []Entry[K, V]) map[K]V
func FromPairs[K comparable, V any](entries []Entry[K, V]) map[K]V // 别名
```

### Invert

交换映射的键和值（若有重复值，后出现的会覆盖先前的）。

```go
result := lo.Invert(map[string]int{"a": 1, "b": 2})
// map[int]string{1: "a", 2: "b"}

func Invert[K comparable, V comparable](in map[K]V) map[V]K
```

### Assign

从左到右合并多个映射，后面的值会覆盖前面的键。

```go
merged := lo.Assign(map[string]int{"a": 1, "b": 2}, map[string]int{"b": 3, "c": 4})
// map[string]int{"a": 1, "b": 3, "c": 4}

func Assign[K comparable, V any, Map ~map[K]V](maps ...Map) Map
```

### MapKeys

使用迭代函数转换映射的所有键，同时保留值。

```go
in := map[int]int{1: 1, 2: 2}
out := lo.MapKeys(in, func(v int, k int) string {
    return strconv.Itoa(k)
})
// map[string]int{"1": 1, "2": 2}

func MapKeys[K comparable, V any, R comparable](in map[K]V, iteratee func(value V, key K) R) map[R]V
```

### MapValues

使用迭代函数转换映射的所有值，同时保留键。

```go
in := map[int]int64{1: 1, 2: 2}
out := lo.MapValues(in, func(v int64, k int) string {
    return strconv.FormatInt(v, 10)
})
// map[int]string{1: "1", 2: "2"}

func MapValues[K comparable, V any, R any](in map[K]V, iteratee func(value V, key K) R) map[K]R
```

### MapEntries

使用迭代函数同时转换映射的键和值。

```go
in := map[string]int{"foo": 1, "bar": 2}
out := lo.MapEntries(in, func(k string, v int) (int, string) {
    return v, k
})
// map[int]string{1: "foo", 2: "bar"}

func MapEntries[K1 comparable, V1 any, K2 comparable, V2 any](in map[K1]V1, iteratee func(key K1, value V1) (K2, V2)) map[K2]V2
```

### MapToSlice

使用转换函数将映射的每个条目转换为切片中的一个元素。

```go
m := map[int]int64{1: 4, 2: 5, 3: 6}
s := lo.MapToSlice(m, func(k int, v int64) string {
    return fmt.Sprintf("%d_%d", k, v)
})
// []string{"1_4", "2_5", "3_6"}

func MapToSlice[K comparable, V any, R any](in map[K]V, iteratee func(key K, value V) R) []R
```

### ChunkEntries

将映射拆分为多个指定最大尺寸的较小映射。

```go
chunks := lo.ChunkEntries(
    map[string]int{"a": 1, "b": 2, "c": 3, "d": 4, "e": 5},
    3,
)
// []map[string]int{
//     {"a": 1, "b": 2, "c": 3},
//     {"d": 4, "e": 5},
// }

func ChunkEntries[K comparable, V any](m map[K]V, size int) []map[K]V
```

> 说明：上述许多函数还有带 `Err` 后缀的变体（如 `PickByErr`、`OmitByErr`、`MapKeysErr`、`MapValuesErr`、`MapEntriesErr`、`MapToSliceErr`），其迭代/谓词函数可额外返回 `error`，一旦出错立即中断并返回该错误。例如：
>
> ```go
> out, err := lo.MapValuesErr(map[int]int64{1: 1, 2: 2}, func(v int64, _ int) (string, error) {
>     if v == 2 {
>         return "", fmt.Errorf("even not allowed")
>     }
>     return strconv.FormatInt(v, 10), nil
> })
> // nil, error("even not allowed")
> ```

---

## 通道操作

### ChannelDispatcher

根据指定策略将消息流分发到多个通道。

```go
stream := make(chan int, 100)
for i := 0; i < 100; i++ {
    stream <- i
}
close(stream)
channels := lo.ChannelDispatcher(stream, 3, 10, lo.DispatchingStrategyRoundRobin[int])
// 返回 3 个通道，采用轮询分发

func ChannelDispatcher[T any](stream <-chan T, count, channelBufferCap int, strategy DispatchingStrategy[T]) []<-chan T
```

### SliceToChannel

将切片转换为指定缓冲大小的通道。

```go
items := []int{1, 2, 3, 4, 5}
ch := lo.SliceToChannel(10, items)
for item := range ch {
    fmt.Println(item)
}

func SliceToChannel[T any](bufferSize int, collection []T) <-chan T
```

### ChannelToSlice

将通道转换为切片。

```go
ch := make(chan int, 3)
ch <- 1
ch <- 2
ch <- 3
close(ch)
slice := lo.ChannelToSlice(ch)
// slice 包含 [1, 2, 3]

func ChannelToSlice[T any](ch <-chan T) []T
```

### Generator

从生成器回调函数创建通道。

```go
gen := lo.Generator(10, func(yield func(int)) {
    for i := 0; i < 10; i++ {
        yield(i * 2)
    }
})
for item := range gen {
    fmt.Println(item)
}
// 打印偶数 0, 2, 4, 6, 8, ...

func Generator[T any](bufferSize int, generator func(yield func(T))) <-chan T
```

### FanIn

将多个上游通道合并为单个下游通道。

```go
merged := lo.FanIn(10, ch1, ch2)
// 顺序可能不固定

func FanIn[T any](channelBufferCap int, upstreams ...<-chan T) <-chan T
```

### FanOut

将单个通道拆分为多个通道。

```go
downstreams := lo.FanOut(3, 10, upstream)
// 返回 3 个通道

func FanOut[T any](count, channelsBufferCap int, upstream <-chan T) []<-chan T
```

### Buffer

从通道读取最多指定数量的元素，返回切片。

```go
items, length, readTime, ok := lo.Buffer(ch, 3)
// items: []int{1, 2, 3}; length: 3; ok: true（通道已关闭）

func Buffer[T any](ch <-chan T, size int) (collection []T, length int, readTime time.Duration, ok bool)
```

### BufferWithContext

从通道读取元素，支持 context 取消。

```go
items, length, readTime, ok := lo.BufferWithContext(ctx, ch, 5)

func BufferWithContext[T any](ctx context.Context, ch <-chan T, size int) (collection []T, length int, readTime time.Duration, ok bool)
```

### BufferWithTimeout

从通道读取元素，带超时限制。

```go
items, length, readTime, ok := lo.BufferWithTimeout(ch, 5, 100*time.Millisecond)
// 超时则返回空切片

func BufferWithTimeout[T any](ch <-chan T, size int, timeout time.Duration) (collection []T, length int, readTime time.Duration, ok bool)
```

### DispatchingStrategy

消息分发策略，作为 `ChannelDispatcher` 的最后一个参数传入，决定每条消息进入哪个通道。共六种内置变体：

- **RoundRobin（轮询）**：依次循环分发到各通道。
- **Random（随机）**：随机选择一个通道。
- **WeightedRandom（加权随机）**：按给定权重随机选择。
- **First（首个）**：分发到第一个未满的通道。
- **Least（最少）**：分发到当前积压最少的通道。
- **Most（最多）**：分发到当前积压最多的通道。

```go
// 轮询分发到 3 个通道
channels := lo.ChannelDispatcher(stream, 3, 10, lo.DispatchingStrategyRoundRobin[int])

// 加权随机：第 3 个通道获得消息的概率最高
strategy := lo.DispatchingStrategyWeightedRandom[int]([]int{1, 3, 6})
channels = lo.ChannelDispatcher(stream, 3, 10, strategy)

func DispatchingStrategyRoundRobin[T any](msg T, index uint64, channels []<-chan T) int
func DispatchingStrategyRandom[T any](msg T, index uint64, channels []<-chan T) int
func DispatchingStrategyWeightedRandom[T any](weights []int) DispatchingStrategy[T]
func DispatchingStrategyFirst[T any](msg T, index uint64, channels []<-chan T) int
func DispatchingStrategyLeast[T any](msg T, index uint64, channels []<-chan T) int
func DispatchingStrategyMost[T any](msg T, index uint64, channels []<-chan T) int
```

---

## 查找操作

### IndexOf

返回值在切片中首次出现的索引，未找到则返回 -1。

```go
idx := lo.IndexOf([]int{0, 1, 2, 1, 2, 3}, 2) // 2
idx = lo.IndexOf([]int{0, 1, 2, 1, 2, 3}, 6)  // -1

func IndexOf[T comparable](collection []T, element T) int
```

### LastIndexOf

返回值在切片中最后出现的索引，未找到则返回 -1。

```go
idx := lo.LastIndexOf([]int{0, 1, 2, 1, 2, 3}, 2) // 4

func LastIndexOf[T comparable](collection []T, element T) int
```

### HasPrefix

如果集合以给定前缀切片开头则返回 true。

```go
lo.HasPrefix([]int{1, 2, 3, 4}, []int{1, 2}) // true

func HasPrefix[T comparable](collection []T, prefix []T) bool
```

### HasSuffix

如果集合以给定后缀切片结尾则返回 true。

```go
lo.HasSuffix([]int{1, 2, 3, 4}, []int{3, 4}) // true

func HasSuffix[T comparable](collection []T, suffix []T) bool
```

### Find

根据谓词在切片中搜索元素，返回元素及表示是否成功的布尔值。

```go
value, ok := lo.Find([]string{"a", "b", "c", "d"}, func(i string) bool {
    return i == "b"
}) // "b", true

func Find[T any](collection []T, predicate func(item T) bool) (T, bool)
```

### FindErr

根据可返回错误的谓词搜索元素。

```go
result, err := lo.FindErr([]string{"a", "b", "c", "d"}, func(i string) (bool, error) {
    return i == "b", nil
}) // "b", nil

func FindErr[T any](collection []T, predicate func(item T) (bool, error)) (T, error)
```

### FindIndexOf

根据谓词搜索元素，返回元素、索引和布尔值。

```go
val, idx, ok := lo.FindIndexOf([]string{"a", "b", "a", "b"}, func(i string) bool {
    return i == "b"
}) // "b", 1, true

func FindIndexOf[T any](collection []T, predicate func(item T) bool) (T, int, bool)
```

### FindLastIndexOf

搜索匹配谓词的最后一个元素，返回元素、索引和布尔值。

```go
val, idx, ok := lo.FindLastIndexOf([]string{"a", "b", "a", "b"}, func(i string) bool {
    return i == "b"
}) // "b", 3, true

func FindLastIndexOf[T any](collection []T, predicate func(item T) bool) (T, int, bool)
```

### FindOrElse

根据谓词搜索元素，找到则返回该元素，否则返回回退值。

```go
value := lo.FindOrElse([]string{"a", "b", "c", "d"}, "x", func(i string) bool {
    return i == "b"
}) // "b"

func FindOrElse[T any](collection []T, fallback T, predicate func(item T) bool) T
```

### FindKey

返回第一个值等于给定值的键。

```go
k, ok := lo.FindKey(map[string]int{"foo":1, "bar":2, "baz":3}, 2) // "bar", true

func FindKey[K comparable, V comparable](object map[K]V, value V) (K, bool)
```

### FindKeyBy

返回映射中第一个使谓词返回 true 的键。

```go
k, ok := lo.FindKeyBy(map[string]int{"foo":1, "bar":2, "baz":3}, func(k string, v int) bool {
    return k == "foo"
}) // "foo", true

func FindKeyBy[K comparable, V any](object map[K]V, predicate func(key K, value V) bool) (K, bool)
```

### FindUniques

返回集合中只出现一次的元素切片（保留原始顺序）。

```go
lo.FindUniques([]int{1, 2, 2, 1, 2, 3}) // []int{3}

func FindUniques[T comparable, Slice ~[]T](collection Slice) Slice
```

### FindUniquesBy

返回按计算键唯一的元素切片。

```go
lo.FindUniquesBy([]int{3, 4, 5, 6, 7}, func(i int) int {
    return i % 3
}) // []int{5}

func FindUniquesBy[T any, U comparable, Slice ~[]T](collection Slice, iteratee func(item T) U) Slice
```

### FindDuplicates

返回集合中每个重复元素首次出现的切片（保留顺序）。

```go
lo.FindDuplicates([]int{1, 2, 2, 1, 2, 3}) // []int{1, 2}

func FindDuplicates[T comparable, Slice ~[]T](collection Slice) Slice
```

### FindDuplicatesBy

返回按键计算的每个重复元素首次出现的切片。

```go
lo.FindDuplicatesBy([]int{3, 4, 5, 6, 7}, func(i int) int {
    return i % 3
}) // []int{3, 4}

func FindDuplicatesBy[T any, U comparable, Slice ~[]T](collection Slice, iteratee func(item T) U) Slice
```

### FindDuplicatesByErr

按键返回重复元素切片，支持错误处理。

```go
result, err := lo.FindDuplicatesByErr([]int{3, 4, 5, 6, 7}, func(i int) (int, error) {
    return i % 3, nil
}) // []int{3, 4}, <nil>

func FindDuplicatesByErr[T any, U comparable, Slice ~[]T](collection Slice, iteratee func(item T) (U, error)) (Slice, error)
```

### Min

返回集合的最小值。

```go
lo.Min([]int{1, 2, 3}) // 1

func Min[T constraints.Ordered](collection []T) T
```

### MinIndex

返回最小值及其索引。

```go
value, idx := lo.MinIndex([]int{2, 5, 3}) // value == 2, idx == 0

func MinIndex[T constraints.Ordered](collection []T) (T, int)
```

### MinBy

使用给定比较函数搜索集合的最小值。

```go
type Point struct{ X int }
min := lo.MinBy([]Point{{1}, {5}, {3}}, func(a, b Point) bool {
    return a.X < b.X
}) // {1}

func MinBy[T any](collection []T, comparison func(a T, b T) bool) T
```

### MinByErr

使用带错误处理的比较函数搜索集合的最小值；比较函数返回错误时立即中断。

```go
type Point struct{ X int }
min, err := lo.MinByErr([]Point{{1}, {5}, {3}}, func(a, b Point) (bool, error) {
    return a.X < b.X, nil
}) // {1}, <nil>

func MinByErr[T any](collection []T, comparison func(a T, b T) (bool, error)) (T, error)
```

### MinIndexBy

使用比较函数搜索最小值，返回值及其索引。

```go
type Point struct{ X int }
value, idx := lo.MinIndexBy([]Point{{1}, {5}, {3}}, func(a, b Point) bool {
    return a.X < b.X
}) // value == {1}, idx == 0

func MinIndexBy[T any](collection []T, comparison func(a T, b T) bool) (T, int)
```

### MinIndexByErr

使用带错误处理的比较函数搜索最小值，返回值、索引和错误。

```go
type Point struct{ X int }
val, idx, err := lo.MinIndexByErr([]Point{{1}, {5}, {3}}, func(a, b Point) (bool, error) {
    return a.X < b.X, nil
}) // val == {1}, idx == 0, err == nil

func MinIndexByErr[T any](collection []T, comparison func(a T, b T) (bool, error)) (T, int, error)
```

### Max

搜索集合的最大值。

```go
max := lo.Max([]int{2, 5, 3}) // 5

func Max[T constraints.Ordered](collection []T) T
```

### MaxIndex

返回最大值及其索引。

```go
value, idx := lo.MaxIndex([]int{2, 5, 3}) // value == 5, idx == 1

func MaxIndex[T constraints.Ordered](collection []T) (T, int)
```

### MaxBy

使用给定比较函数搜索集合的最大值。

```go
type Point struct{ X int }
max := lo.MaxBy([]Point{{1}, {5}, {3}}, func(a, b Point) bool {
    return a.X > b.X
}) // {5}

func MaxBy[T any](collection []T, comparison func(a T, b T) bool) T
```

### MaxByErr

使用带错误处理的比较函数搜索集合的最大值；比较函数返回错误时立即中断。

```go
type Point struct{ X int }
max, err := lo.MaxByErr([]Point{{1}, {5}, {3}}, func(a, b Point) (bool, error) {
    return a.X > b.X, nil
}) // {5}, <nil>

func MaxByErr[T any](collection []T, comparison func(a T, b T) (bool, error)) (T, error)
```

### MaxIndexBy

使用给定比较函数返回最大值及其索引。

```go
type Point struct{ X int }
value, idx := lo.MaxIndexBy([]Point{{1}, {5}, {3}}, func(a, b Point) bool {
    return a.X > b.X
}) // value == {5}, idx == 1

func MaxIndexBy[T any](collection []T, comparison func(a T, b T) bool) (T, int)
```

### MaxIndexByErr

带错误处理返回最大值及其索引。

```go
type Point struct{ X int }
val, idx, err := lo.MaxIndexByErr([]Point{{1}, {5}, {3}}, func(a, b Point) (bool, error) {
    return a.X > b.X, nil
}) // val == {5}, idx == 1, err == nil

func MaxIndexByErr[T any](collection []T, comparison func(a T, b T) (bool, error)) (T, int, error)
```

### Earliest

搜索提供参数中最早的 time.Time。

```go
t1 := time.Date(2023, 1, 1, 0, 0, 0, 0, time.UTC)
t2 := time.Date(2024, 1, 1, 0, 0, 0, 0, time.UTC)
min := lo.Earliest(t2, t1) // 2023-01-01 00:00:00 +0000 UTC

func Earliest(times ...time.Time) time.Time
```

### EarliestBy

在集合中搜索由谓词提取出最早时间的元素。

```go
type Event struct{ At time.Time }
first := lo.EarliestBy(events, func(e Event) time.Time {
    return e.At
})

func EarliestBy[T any](collection []T, iteratee func(item T) time.Time) T
```

### EarliestByErr

在集合中搜索由迭代函数提取出最早时间的元素，支持错误处理。

```go
type Event struct{ At time.Time }
events := []Event{{At: time.Now().Add(2 * time.Hour)}, {At: time.Now()}}
first, err := lo.EarliestByErr(events, func(e Event) (time.Time, error) {
    return e.At, nil
}) // 时间最早的 Event, <nil>

func EarliestByErr[T any](collection []T, iteratee func(item T) (time.Time, error)) (T, error)
```

### Latest

搜索提供参数中最晚的 time.Time。

```go
max := lo.Latest(t1, t2) // 2024-01-01 00:00:00 +0000 UTC

func Latest(times ...time.Time) time.Time
```

### LatestBy

在集合中搜索由谓词提取出最晚时间的元素。

```go
last := lo.LatestBy(events, func(e Event) time.Time {
    return e.At
})

func LatestBy[T any](collection []T, iteratee func(item T) time.Time) T
```

### LatestByErr

在集合中搜索由迭代函数提取出最晚时间的元素，支持错误处理。

```go
type Event struct{ At time.Time }
events := []Event{{At: time.Now()}, {At: time.Now().Add(2 * time.Hour)}}
last, err := lo.LatestByErr(events, func(e Event) (time.Time, error) {
    return e.At, nil
}) // 时间最晚的 Event, <nil>

func LatestByErr[T any](collection []T, iteratee func(item T) (time.Time, error)) (T, error)
```

### First

返回集合的第一个元素及其是否存在。

```go
v, ok := lo.First([]int{1, 2, 3}) // v == 1, ok == true

func First[T any](collection []T) (T, bool)
```

### FirstOrEmpty

返回集合的第一个元素，若为空则返回零值。

```go
v := lo.FirstOrEmpty([]int{}) // v == 0

func FirstOrEmpty[T any](collection []T) T
```

### FirstOr

返回集合的第一个元素，若为空则返回回退值。

```go
v := lo.FirstOr([]int{}, -1) // v == -1

func FirstOr[T any](collection []T, fallback T) T
```

### Last

返回集合的最后一个元素及其是否存在。

```go
v, ok := lo.Last([]int{1, 2, 3}) // v == 3, ok == true

func Last[T any](collection []T) (T, bool)
```

### LastOrEmpty

返回集合的最后一个元素，若为空则返回零值。

```go
v := lo.LastOrEmpty([]int{}) // v == 0

func LastOrEmpty[T any](collection []T) T
```

### LastOr

返回集合的最后一个元素，若为空则返回回退值。

```go
v := lo.LastOr([]int{}, -1) // v == -1

func LastOr[T any](collection []T, fallback T) T
```

### Nth

返回集合中索引为 nth 的元素，支持负索引。

```go
v, _ := lo.Nth([]int{10, 20, 30}, 1) // v == 20

func Nth[T any, N constraints.Integer](collection []T, nth N) (T, error)
```

### NthOr

返回集合中索引为 nth 的元素，越界则返回回退值。

```go
v := lo.NthOr([]int{10, 20, 30}, 10, -1) // v == -1

func NthOr[T any, N constraints.Integer](collection []T, nth N, fallback T) T
```

### NthOrEmpty

返回集合中索引为 nth 的元素，越界则返回零值。

```go
v := lo.NthOrEmpty([]int{10, 20, 30}, 10) // v == 0

func NthOrEmpty[T any, N constraints.Integer](collection []T, nth N) T
```

### Sample

从集合中返回一个随机元素。

```go
v := lo.Sample([]int{10, 20, 30})

func Sample[T any](collection []T) T
```

### SampleBy

使用提供的随机索引生成器从集合中返回一个随机元素。

```go
v := lo.SampleBy([]int{10, 20, 30}, func(n int) int {
    return 0
}) // v == 10

func SampleBy[T any](collection []T, randomIntGenerator func(int) int) T
```

### Samples

从集合中返回 N 个不重复的随机元素。

```go
v := lo.Samples([]int{10, 20, 30}, 2)

func Samples[T any, Slice ~[]T](collection Slice, count int) Slice
```

### SamplesBy

使用提供的随机索引生成器从集合中返回 N 个不重复的随机元素。

```go
v := lo.SamplesBy([]int{10, 20, 30}, 2, func(n int) int {
    return 0
})

func SamplesBy[T any, Slice ~[]T](collection Slice, count int, randomIntGenerator func(int) int) Slice
```

---

## 字符串操作

### RandomString

使用给定字符集生成指定长度的随机文本。

```go
lo.RandomString(5, lo.LettersCharset) // 返回随机的 5 个字符

func RandomString(size int, charset []rune) string
```

### Substring

从指定偏移量开始提取指定长度的文本片段，支持负索引。

```go
lo.Substring("hello", 2, 3) // "llo"

func Substring[T ~string](str T, offset int, length uint) T
```

### ChunkString

将文本拆分为等长的片段。

```go
lo.ChunkString("1234567", 2) // []string{"12", "34", "56", "7"}

func ChunkString[T ~string](str T, size int) []T
```

### RuneLength

统计 Unicode 字符数量（而非字节数）。

```go
lo.RuneLength("hellô") // 5

func RuneLength(str string) int
```

### PascalCase

转换为 PascalCase（首字母大写驼峰）格式。

```go
lo.PascalCase("hello_world") // "HelloWorld"

func PascalCase(str string) string
```

### CamelCase

转换为 camelCase（小驼峰）格式。

```go
lo.CamelCase("hello_world") // "helloWorld"

func CamelCase(str string) string
```

### KebabCase

转换为 kebab-case（连字符分隔）格式。

```go
lo.KebabCase("helloWorld") // "hello-world"

func KebabCase(str string) string
```

### SnakeCase

转换为 snake_case（下划线分隔）格式。

```go
lo.SnakeCase("HelloWorld") // "hello_world"

func SnakeCase(str string) string
```

### Words

将文本拆分为单词列表。

```go
lo.Words("helloWorld") // []string{"hello", "world"}

func Words(str string) []string
```

### Capitalize

将首字母大写，其余字母小写。

```go
lo.Capitalize("heLLO") // "Hello"

func Capitalize(str string) string
```

### Ellipsis

将文本截断到指定长度并在需要时追加 "..."，保留多字节字符。

```go
lo.Ellipsis("Lorem Ipsum", 5) // "Lo..."

func Ellipsis(str string, length int) string
```

---

## 函数操作

### Partial / PartialX

预绑定函数的第一个参数。Partial1 到 Partial5 变体支持 2 到 6 个输入参数的函数。

```go
add := func(x, y int) int { return x + y }
add10 := lo.Partial(add, 10)
sum := add10(5) // 15

func Partial[T1, T2, R any](f func(a T1, b T2) R, arg1 T1) func(T2) R
func Partial1[T1, T2, R any](f func(T1, T2) R, arg1 T1) func(T2) R
func Partial2[T1, T2, T3, R any](f func(T1, T2, T3) R, arg1 T1) func(T2, T3) R
// ... Partial3 到 Partial5 变体类似
```

---

## 条件操作

### Ternary

单行 if/else：条件为 true 时返回 `ifOutput`，否则返回 `elseOutput`。

```go
result := lo.Ternary(true, "a", "b") // "a"
age := 25
status := lo.Ternary(age >= 18, "adult", "minor") // "adult"

func Ternary[T any](condition bool, ifOutput T, elseOutput T) T
```

### TernaryF

函数变体，仅惰性求值所需分支，避免不必要的计算或 nil 解引用。

```go
str := lo.TernaryF(s != nil, func() string {
    return *s
}, func() string {
    return "default value"
})

func TernaryF[T any](condition bool, ifFunc func() T, elseFunc func() T) T
```

### If / ElseIf / Else

流式条件构建器，允许链式调用 If/ElseIf/Else 条件。

```go
result := lo.If(false, 1).ElseIf(true, 2).Else(3) // 2

func If[T any](condition bool, result T) *ifElse[T]
func (i *ifElse[T]) ElseIf(condition bool, result T) *ifElse[T]
func (i *ifElse[T]) Else(result T) T
```

### Switch / Case / Default

使用谓词值发起函数式 switch/case/default 链。

```go
result := lo.Switch(2).Case(1, "1").Case(2, "2").Default("3") // "2"

func Switch[T comparable, R any](predicate T) *switchCase[T, R]
func (s *switchCase[T, R]) Case(val T, result R) *switchCase[T, R]
func (s *switchCase[T, R]) Default(result R) R
```

---

## 数学操作

### Range

创建从 0 开始的整数切片；负长度产生递减序列。

```go
lo.Range(4) // []int{0, 1, 2, 3}

func Range(elementNum int) []int
```

### RangeFrom

从指定起始值开始，按定义长度生成序列。

```go
lo.RangeFrom(1, 5) // []int{1, 2, 3, 4, 5}

func RangeFrom[T constraints.Integer | constraints.Float](start T, elementNum int) []T
```

### RangeWithSteps

从 start 到 end（不含）按 step 步进生成序列；step 为 0 时返回空。

```go
lo.RangeWithSteps(0, 20, 5) // []int{0, 5, 10, 15}

func RangeWithSteps[T constraints.Integer | constraints.Float](start, end, step T) []T
```

### Clamp

将值约束在指定的闭区间边界内。

```go
lo.Clamp(42, -10, 10) // 10

func Clamp[T constraints.Ordered](value T, min T, max T) T
```

### Sum

聚合集合中的值；空集合返回 0。

```go
lo.Sum([]int{1, 2, 3, 4, 5}) // 15

func Sum[T constraints.Float | constraints.Integer | constraints.Complex](collection []T) T
```

### SumBy

通过谓词函数计算总和。

```go
lo.SumBy([]string{"foo", "bar"}, func(item string) int { return len(item) }) // 6

func SumBy[T any, R constraints.Float | constraints.Integer | constraints.Complex](collection []T, iteratee func(item T) R) R
```

### Product

将集合元素相乘；空集合默认返回 1。

```go
lo.Product([]int{1, 2, 3, 4, 5}) // 120

func Product[T constraints.Float | constraints.Integer | constraints.Complex](collection []T) T
```

### ProductBy

通过迭代函数将每个元素映射为数值后计算乘积。

```go
lo.ProductBy([]string{"foo", "bar"}, func(item string) int {
    return len(item)
}) // 9  （3 × 3）

func ProductBy[T any, R constraints.Float | constraints.Integer | constraints.Complex](collection []T, iteratee func(item T) R) R
```

### Mean

计算算术平均值；空集合返回 0。

```go
lo.Mean([]int{2, 3, 4, 5}) // 3

func Mean[T constraints.Float | constraints.Integer](collection []T) T
```

### MeanBy

通过迭代函数将每个元素映射为数值后计算算术平均值。

```go
list := []string{"aa", "bbb", "cccc", "ddddd"}
lo.MeanBy(list, func(item string) float64 {
    return float64(len(item))
}) // 3.5  （(2+3+4+5)/4）

func MeanBy[T any, R constraints.Float | constraints.Integer](collection []T, iteratee func(item T) R) R
```

### Mode

找出出现频率最高的值；返回所有并列的值。

```go
lo.Mode([]int{2, 2, 3, 3}) // []int{2, 3}

func Mode[T constraints.Integer | constraints.Float](collection []T) []T
```

---

## 交集操作

### Contains

判断元素是否存在于集合中。

```go
present := lo.Contains([]int{0, 1, 2, 3, 4, 5}, 5) // true

func Contains[T comparable](collection []T, element T) bool
```

### ContainsBy

判断谓词是否对集合中任一元素返回 true。

```go
exists := lo.ContainsBy([]int{0, 1, 2, 3}, func(x int) bool { return x == 3 }) // true

func ContainsBy[T any](collection []T, predicate func(item T) bool) bool
```

### Every

验证子集的所有元素是否都包含在集合中。

```go
ok := lo.Every([]int{0, 1, 2, 3, 4, 5}, []int{0, 2}) // true

func Every[T comparable](collection []T, subset []T) bool
```

### EveryBy

如果谓词对所有元素都成立（或集合为空）则返回 true。

```go
ok := lo.EveryBy([]int{1, 2, 3, 4}, func(x int) bool { return x < 5 }) // true

func EveryBy[T any](collection []T, predicate func(item T) bool) bool
```

### Some

判断子集中是否至少有一个元素存在于集合中。

```go
ok := lo.Some([]int{0, 1, 2, 3, 4, 5}, []int{0, 6}) // true

func Some[T comparable](collection []T, subset []T) bool
```

### SomeBy

如果谓词匹配任一元素则返回 true。

```go
ok := lo.SomeBy([]int{1, 2, 3, 4}, func(x int) bool { return x < 3 }) // true

func SomeBy[T any](collection []T, predicate func(item T) bool) bool
```

### None

确认子集中没有元素出现在集合中。

```go
ok := lo.None([]int{0, 1, 2, 3, 4, 5}, []int{-1, 6}) // true

func None[T comparable](collection []T, subset []T) bool
```

### NoneBy

如果谓词对所有元素都不成立（或集合为空）则返回 true。

```go
ok := lo.NoneBy([]int{1, 2, 3, 4}, func(x int) bool { return x < 0 }) // true

func NoneBy[T any](collection []T, predicate func(item T) bool) bool
```

### Intersect

计算多个集合之间的交集。

```go
lo.Intersect([]int{0, 3, 5, 7}, []int{3, 5}, []int{0, 1, 2, 0, 3, 0}) // []int{3}

func Intersect[T comparable, Slice ~[]T](lists ...Slice) Slice
```

### IntersectBy

使用自定义键转换函数计算多个集合的交集（按转换后的键比较）。

```go
transform := func(v int) string { return strconv.Itoa(v) }
lo.IntersectBy(transform, []int{0, 3, 5, 7}, []int{3, 5}, []int{0, 1, 2, 0, 3, 0})
// []int{3}

func IntersectBy[T any, K comparable, Slice ~[]T](transform func(T) K, lists ...Slice) Slice
```

### Difference

返回两个集合各自独有的元素（分别返回）。

```go
left, right := lo.Difference([]int{0, 1, 2, 3, 4, 5}, []int{0, 2, 6})
// ([]int{1, 3, 4, 5}, []int{6})

func Difference[T comparable, Slice ~[]T](list1 Slice, list2 Slice) (Slice, Slice)
```

### Union

合并所有集合中的不重复元素，保留顺序。

```go
lo.Union([]int{0, 1, 2, 3, 4, 5}, []int{0, 2}, []int{0, 10})
// []int{0, 1, 2, 3, 4, 5, 10}

func Union[T comparable, Slice ~[]T](lists ...Slice) Slice
```

### UnionBy

基于键函数合并集合中的不重复元素（按计算键去重），保留首次出现的顺序。

```go
lo.UnionBy(func(i int) int { return i / 2 }, []int{0, 1, 2, 3, 4, 5}, []int{0, 2, 10})
// []int{0, 2, 4, 10}

func UnionBy[T any, V comparable, Slice ~[]T](iteratee func(item T) V, lists ...Slice) Slice
```

### UnionByErr

与 UnionBy 类似，但键函数可返回错误，遇错即停。

```go
result, err := lo.UnionByErr(
    func(i int) (int, error) { return i / 2, nil },
    []int{0, 1, 2, 3, 4, 5}, []int{0, 2, 10},
) // []int{0, 2, 4, 10}, nil

func UnionByErr[T any, V comparable, Slice ~[]T](iteratee func(item T) (V, error), lists ...Slice) (Slice, error)
```

### Without

从集合中排除指定的值。

```go
lo.Without([]int{0, 2, 10}, 2) // []int{0, 10}

func Without[T comparable, Slice ~[]T](collection Slice, exclude ...T) Slice
```

### WithoutBy

从集合中排除「提取键」命中排除列表的元素，适用于结构体切片按某字段过滤。

```go
type User struct {
    ID   int
    Name string
}
users := []User{{1, "Alice"}, {2, "Bob"}, {3, "Charlie"}}
lo.WithoutBy(users, func(u User) int { return u.ID }, 2, 3)
// []User{{1, "Alice"}}

func WithoutBy[T any, K comparable, Slice ~[]T](collection Slice, iteratee func(item T) K, exclude ...K) Slice
```

### WithoutByErr

按键函数排除元素，支持错误处理；键函数返回错误时立即中断。

```go
type User struct {
    ID   int
    Name string
}
users := []User{{1, "Alice"}, {2, "Bob"}, {3, "Charlie"}}
filtered, err := lo.WithoutByErr(users, func(u User) (int, error) {
    return u.ID, nil
}, 2, 3) // []User{{1, "Alice"}}, nil

func WithoutByErr[T any, K comparable, Slice ~[]T](collection Slice, iteratee func(item T) (K, error), exclude ...K) (Slice, error)
```

### WithoutEmpty

移除零值；已弃用，推荐使用 Compact。

```go
lo.WithoutEmpty([]int{0, 2, 10}) // []int{2, 10}

func WithoutEmpty[T comparable, Slice ~[]T](collection Slice) Slice
```

### WithoutNth

排除指定索引处的元素。

```go
lo.WithoutNth([]int{-2, -1, 0, 1, 2}, 3) // []int{-2, -1, 0, 2}

func WithoutNth[T comparable, Slice ~[]T](collection Slice, nths ...int) Slice
```

### ElementsMatch

如果两个列表包含相同的元素集合（含相同重复次数，忽略顺序）则返回 true。

```go
lo.ElementsMatch([]int{1, 1, 2}, []int{2, 1, 1}) // true

func ElementsMatch[T comparable, Slice ~[]T](list1 Slice, list2 Slice) bool
```

### ElementsMatchBy

使用计算键而非直接元素比较两个列表是否包含相同的元素集合（含相同重复次数，忽略顺序）。

```go
type Item struct{ ID string }
lo.ElementsMatchBy(
    []Item{{"a"}, {"b"}},
    []Item{{"b"}, {"a"}},
    func(i Item) string { return i.ID },
) // true

func ElementsMatchBy[T any, K comparable](list1 []T, list2 []T, iteratee func(item T) K) bool
```

---

## 重试操作

### Attempt

执行函数最多 N 次直到成功。返回迭代次数（从 1 开始计数）和最终错误。

```go
iter, err := lo.Attempt(3, func(index int) error {
    // 第三次尝试成功
    return nil
}) // iter: 3, err: nil

func Attempt(maxIteration int, f func(index int) error) (int, error)
```

### AttemptWithDelay

与 Attempt 类似，但在每次调用之间插入固定延迟。返回迭代次数、总耗时和最终错误。

```go
iter, dur, err := lo.AttemptWithDelay(3, 100*time.Millisecond,
    func(i int, d time.Duration) error {
        if i == 1 {
            return nil // 第二次尝试成功
        }
        return errors.New("失败")
    },
) // iter: 2, dur: ~100ms, err: nil

func AttemptWithDelay(maxIteration int, delay time.Duration,
    f func(index int, duration time.Duration) error) (int, time.Duration, error)
```

### AttemptWhile

重试直到返回 nil 或达到最大迭代次数。回调的第二个返回值（bool）决定是否继续重试：返回 false 则立即停止。

```go
count, err := lo.AttemptWhile(5, func(i int) (error, bool) {
    if i == 2 {
        return nil, false // 第三次成功并停止
    }
    return errors.New("失败"), true // 继续重试
}) // count: 3, err: nil

func AttemptWhile(maxIteration int, f func(int) (error, bool)) (int, error)
```

### AttemptWhileWithDelay

结合 AttemptWhile 与延迟行为，在每次重试间插入延迟并跟踪总耗时。

```go
count, dur, err := lo.AttemptWhileWithDelay(5, time.Millisecond,
    func(i int, d time.Duration) (error, bool) {
        return errors.New("错误"), i < 1 // 仅重试一次后停止
    },
) // count: 2, dur: ~1ms, err: 错误

func AttemptWhileWithDelay(maxIteration int, delay time.Duration,
    f func(int, time.Duration) (error, bool)) (int, time.Duration, error)
```

---

## 并发操作

### NewDebounce

创建防抖函数，将回调延迟到最后一次调用后经过等待时间才执行。返回防抖函数和取消函数。

```go
debounce, cancel := lo.NewDebounce(
    100*time.Millisecond,
    func() {
        println("防抖后只调用一次！")
    },
)
for i := 0; i < 10; i++ {
    debounce()
}
time.Sleep(200 * time.Millisecond)
cancel()

func NewDebounce(duration time.Duration, f ...func()) (func(), func())
```

### NewDebounceBy

按键创建防抖函数，为不同的键跟踪各自独立的计时器；回调可获知该键累计触发的次数 count。

```go
debounce, cancel := lo.NewDebounceBy[string](
    100*time.Millisecond,
    func(key string, count int) {
        println(key, "called", count, "times")
    },
)
for i := 0; i < 5; i++ {
    debounce("action")
}
time.Sleep(200 * time.Millisecond)
// 输出: action called 5 times
cancel("action")

func NewDebounceBy[T comparable](duration time.Duration, f ...func(key T, count int)) (func(key T), func(key T))
```

### Synchronize

用互斥锁包装回调以确保顺序执行，可接受可选的自定义 locker。

```go
s := lo.Synchronize()
for i := 0; i < 10; i++ {
    go s.Do(func() { println("sequential") })
}

func Synchronize(opt ...sync.Locker) *synchronize
```

### Async / AsyncX

异步运行函数并通过通道返回结果（Async0 到 Async6 支持 0-6 个返回值）。

```go
ch := lo.Async(func() int {
    time.Sleep(10 * time.Millisecond)
    return 42
})
v := <-ch

func Async[A any](f func() A) <-chan A
func Async0(f func()) <-chan struct{}
func Async2[A, B any](f func() (A, B)) <-chan Tuple2[A, B]
// ... Async3 到 Async6 变体类似
```

### WaitFor

周期性运行直到条件验证通过，返回迭代次数、耗时和成功状态。包含 context 感知变体。

```go
iterations, elapsed, ok := lo.WaitFor(
    func(i int) bool {
        return i > 5
    },
    10*time.Millisecond,
    time.Millisecond,
)

func WaitFor(condition func(i int) bool, timeout time.Duration, heartbeatDelay time.Duration) (totalIterations int, elapsed time.Duration, conditionFound bool)
func WaitForWithContext(ctx context.Context, condition func(ctx context.Context, i int) bool, timeout time.Duration, heartbeatDelay time.Duration) (totalIterations int, elapsed time.Duration, conditionFound bool)
```

### NewTransaction

创建 Saga 模式事务，串联各步骤并附带回滚函数用于错误处理；失败时按相反顺序回滚。

```go
type Acc struct{ Sum int }
tx := lo.NewTransaction[Acc]().
    Then(
        func(a Acc) (Acc, error) { a.Sum += 10; return a, nil },
        func(a Acc) Acc { a.Sum -= 10; return a },
    ).
    Then(
        func(a Acc) (Acc, error) { a.Sum *= 3; return a, nil },
        func(a Acc) Acc { a.Sum /= 3; return a },
    )
res, err := tx.Process(Acc{Sum: 1})
// res.Sum == 33, err == nil

func NewTransaction[T any]() *Transaction[T]
```

### NewThrottle

创建节流函数，每个时间间隔内最多调用一次回调，并提供 reset 函数。

```go
throttle, reset := lo.NewThrottle(
    100*time.Millisecond,
    func() {
        println("tick")
    },
)

func NewThrottle(interval time.Duration, f ...func()) (throttle func(), reset func())
```

### NewThrottleWithCount

创建节流函数，每个时间间隔内最多调用 count 次回调。

```go
throttle, reset := lo.NewThrottleWithCount(
    100*time.Millisecond,
    2, // 每 100ms 内最多执行 2 次
    func() { println("throttled") },
)
for i := 0; i < 6; i++ {
    throttle()
    time.Sleep(30 * time.Millisecond)
}
reset()

func NewThrottleWithCount(interval time.Duration, count int, f ...func()) (throttle func(), reset func())
```

### NewThrottleBy

按键创建节流函数，相同键在每个时间间隔内最多触发一次。

```go
throttle, reset := lo.NewThrottleBy[string](
    100*time.Millisecond,
    func(key string) { println("key:", key) },
)
for i := 0; i < 5; i++ {
    throttle("user123") // 同一键 "user123" 每 100ms 内只会触发一次
    time.Sleep(25 * time.Millisecond)
}
reset()

func NewThrottleBy[T comparable](interval time.Duration, f ...func(key T)) (throttle func(key T), reset func())
```

### NewThrottleByWithCount

按键创建节流函数，相同键在每个时间间隔内最多触发 count 次。

```go
throttle, reset := lo.NewThrottleByWithCount[string](
    100*time.Millisecond,
    3, // 同一键每 100ms 内最多执行 3 次
    func(key string) { println("key:", key) },
)
for i := 0; i < 10; i++ {
    throttle("event")
    time.Sleep(20 * time.Millisecond)
}
reset()

func NewThrottleByWithCount[T comparable](interval time.Duration, count int, f ...func(key T)) (throttle func(key T), reset func())
```

---

## 元组操作

### T2-T9 (TupleX)

从 2 到 9 个元素创建元组值。

```go
t := lo.T3(1, "a", true)

func T2[A, B any](a A, b B) Tuple2[A, B]
func T3[A, B, C any](a A, b B, c C) Tuple3[A, B, C]
// ... T4 到 T9 变体类似
```

### Unpack2-Unpack9 (UnpackX)

从元组中提取值到独立变量。

```go
a, b, c := lo.Unpack3(lo.T3(1, "a", true))

func Unpack2[A, B any](tuple Tuple2[A, B]) (A, B)
// ... Unpack3 到 Unpack9 变体类似
```

### Zip2-Zip9 (ZipX)

将多个切片压缩为元组切片。

```go
xs := []int{1, 2}
ys := []string{"a", "b"}
pairs := lo.Zip2(xs, ys)

func Zip2[A, B any](a []A, b []B) []Tuple2[A, B]
// ... Zip3 到 Zip9 变体类似
```

### Unzip2-Unzip9 (UnzipX)

将元组切片拆分回多个并行切片（Zip 的逆操作）。

```go
pairs := []lo.Tuple2[int, string]{{A: 1, B: "a"}, {A: 2, B: "b"}}
xs, ys := lo.Unzip2(pairs)

func Unzip2[A, B any](tuples []Tuple2[A, B]) ([]A, []B)
// ... Unzip3 到 Unzip9 变体类似
```

### ZipBy2-ZipBy9 (ZipByX)

压缩多个切片，并对每组对应元素应用转换函数，直接得到结果切片。

```go
out := lo.ZipBy2([]int{1, 2}, []string{"a", "b"}, func(a int, b string) string {
    return fmt.Sprintf("%d%s", a, b)
})
// []string{"1a", "2b"}

func ZipBy2[A any, B any, Out any](a []A, b []B, predicate func(a A, b B) Out) []Out
// ... ZipBy3 到 ZipBy9 变体类似
```

### ZipByErr2-ZipByErr9 (ZipByErrX)

带错误处理的压缩，转换失败时立即停止迭代。

```go
func ZipByErr2[A any, B any, Out any](a []A, b []B, iteratee func(a A, b B) (Out, error)) ([]Out, error)
// ... ZipByErr3 到 ZipByErr9 变体类似
```

### UnzipBy2-UnzipBy9 (UnzipByX)

将每个输入元素通过转换函数拆成多个值，再分别收集为并行切片。

```go
names := []string{"Alice:30", "Bob:25"}
ns, ages := lo.UnzipBy2(names, func(s string) (string, int) {
    parts := strings.Split(s, ":")
    age, _ := strconv.Atoi(parts[1])
    return parts[0], age
})
// ns:   []string{"Alice", "Bob"}
// ages: []int{30, 25}

func UnzipBy2[In any, A any, B any](items []In, predicate func(In) (a A, b B)) ([]A, []B)
// ... UnzipBy3 到 UnzipBy9 变体类似
```

### UnzipByErr2-UnzipByErr9 (UnzipByErrX)

与 UnzipByX 类似，但支持错误处理。

```go
func UnzipByErr2[In any, A any, B any](items []In, predicate func(In) (a A, b B, err error)) ([]A, []B, error)
// ... UnzipByErr3 到 UnzipByErr9 变体类似
```

### CrossJoin2-CrossJoin9 (CrossJoinX)

计算输入切片的笛卡尔积，返回所有可能的组合元组。

```go
a := []int{1, 2}
b := []string{"x", "y"}
pairs := lo.CrossJoin2(a, b)

func CrossJoin2[A, B any](listA []A, listB []B) []Tuple2[A, B]
// ... CrossJoin3 到 CrossJoin9 变体类似
```

### CrossJoinBy2-CrossJoinBy9 (CrossJoinByX)

计算笛卡尔积，并对每个组合应用转换函数投影为结果。

```go
out := lo.CrossJoinBy2([]int{1, 2}, []string{"a", "b"}, func(a int, b string) string {
    return fmt.Sprintf("%d-%s", a, b)
})
// []string{"1-a", "1-b", "2-a", "2-b"}

func CrossJoinBy2[A, B, Out any](listA []A, listB []B, transform func(a A, b B) Out) []Out
// ... CrossJoinBy3 到 CrossJoinBy9 变体类似
```

### CrossJoinByErr2-CrossJoinByErr9 (CrossJoinByErrX)

带错误处理的笛卡尔积。

```go
func CrossJoinByErr2[A any, B any, Out any](listA []A, listB []B, transform func(a A, b B) (Out, error)) ([]Out, error)
// ... CrossJoinByErr3 到 CrossJoinByErr9 变体类似
```

---

## 时间操作

### Duration / DurationX

测量函数的执行时间。各变体在返回耗时的同时返回函数的 0 到 10 个返回值。

```go
// 基础变体 - 仅返回耗时
elapsedOnly := lo.Duration(func() {
    time.Sleep(3 * time.Millisecond)
})

// 零返回值变体
elapsed := lo.Duration0(func() {
    time.Sleep(10 * time.Millisecond)
})

// 一个返回值变体 - 捕获函数结果加耗时
v, dur := lo.Duration1(func() int {
    time.Sleep(5 * time.Millisecond)
    return 123
})

// 两个返回值变体
a, b, elapsed2 := lo.Duration2(func() (int, string) {
    time.Sleep(2 * time.Millisecond)
    return 7, "x"
})

func Duration(cb func()) time.Duration
func Duration0(cb func()) time.Duration
func Duration1[A any](cb func() A) (A, time.Duration)
func Duration2[A, B any](cb func() (A, B)) (A, B, time.Duration)
// ... 直到 Duration10
```

---

## 错误处理

### Validate

创建带格式化消息的条件错误。条件通过时返回 nil。

```go
err := lo.Validate(len(slice) == 0, "Slice should be empty")

func Validate(ok bool, format string, args ...any) error
```

### Must / MustX

如果发生错误则 panic，否则返回成功的值。支持 0-6 个返回值。

```go
v := lo.Must(strconv.Atoi("10"))
a, b, c, d := lo.Must4(myFunc())

func Must[T any](val T, err error) T
func Must0(err error)
// ... Must1 到 Must6 变体类似
```

### Try / TryX

执行函数，发生错误/panic 时返回 false。涵盖 0-6 个返回值的回调。

```go
ok := lo.Try(func() error { return fmt.Errorf("boom") })

func Try(callback func() error) bool
func Try0(callback func()) bool
// ... Try1 到 Try6 变体类似
```

### TryOr / TryOrX

与 Try 类似，但发生错误时返回回退值加成功标志。处理 1-6 个返回值。

```go
value, ok := lo.TryOr(func() (int, error) { return 0, fmt.Errorf("boom") }, 123)

func TryOr[A any](callback func() (A, error), fallbackA A) (A, bool)
// ... TryOr1 到 TryOr6 变体类似
```

### TryWithErrorValue

执行函数并返回错误/panic 值及成功布尔值。

```go
err, ok := lo.TryWithErrorValue(func() error { panic("error") })

func TryWithErrorValue(callback func() error) (errorValue any, ok bool)
```

### TryCatch

在回调发生错误/panic 时调用 catch 处理器。

```go
lo.TryCatch(func() error { panic("error") }, func() { /* 处理 */ })

func TryCatch(callback func() error, catch func())
```

### TryCatchWithErrorValue

与 TryCatch 类似，但将捕获到的错误值/panic 值传递给处理器，便于做针对性处理。

```go
lo.TryCatchWithErrorValue(
    func() error {
        panic("something went wrong!")
    },
    func(val any) {
        fmt.Printf("捕获到: %v\n", val)
        // 输出: 捕获到: something went wrong!
    },
)

func TryCatchWithErrorValue(callback func() error, catch func(any))
```

### ErrorsAs

标准库 `errors.As` 的泛型封装，返回类型化错误和成功标志。

```go
if rateLimitErr, ok := lo.ErrorsAs[*RateLimitError](err); ok {
    // 处理
}

func ErrorsAs[T error](err error) (T, bool)
```

### Assert / Assertf

条件失败时 panic；条件为 true 时不做任何操作。支持可选消息和格式化。

```go
lo.Assert(age >= 15, "user age must be >= 15")

func Assert(condition bool, message ...string)
func Assertf(condition bool, format string, args ...any)
```

---

## 类型操作

### Nil

返回指定类型的 nil 指针。

```go
ptr := lo.Nil[string]() // (*string)(nil)
ptr2 := lo.Nil[int]()   // (*int)(nil)

func Nil[T any]() *T
```

### IsNil

检查输入是否为 nil（适用于指针、接口、映射、切片、通道、函数）。

```go
lo.IsNil(nil) // true
var ptr *int
lo.IsNil(ptr) // true
lo.IsNil(42)  // false

func IsNil(x any) bool
```

### IsNotNil

IsNil 的反向操作。

```go
lo.IsNotNil(nil)     // false
lo.IsNotNil(42)      // true
lo.IsNotNil("hello") // true

func IsNotNil(x any) bool
```

### ToPtr

返回指向所提供值的指针（常用于需要 `*T` 字面量的场景）。

```go
ptr := lo.ToPtr(42)       // *int，指向 42
ptr2 := lo.ToPtr("hello") // *string，指向 "hello"

func ToPtr[T any](x T) *T
```

### FromPtr

解引用指针；若为 nil 则返回零值。

```go
ptr := lo.ToPtr(42)
lo.FromPtr(ptr)        // 42
lo.FromPtr[string](nil) // ""

func FromPtr[T any](x *T) T
```

### FromPtrOr

返回指针指向的值，若为 nil 则返回回退值。

```go
ptr := lo.ToPtr(42)
lo.FromPtrOr(ptr, 0)      // 42
lo.FromPtrOr[int](nil, -1) // -1

func FromPtrOr[T any](x *T, fallback T) T
```

### EmptyableToPtr

仅当值非零值时才转换为指针，零值时返回 nil。

```go
lo.EmptyableToPtr("")      // nil
lo.EmptyableToPtr("hello") // *string，指向 "hello"
lo.EmptyableToPtr(0)       // nil

func EmptyableToPtr[T any](x T) *T
```

### ToSlicePtr

将值切片转换为指针切片。

```go
slice := []int{1, 2, 3}
ptrs := lo.ToSlicePtr(slice) // []*int{&1, &2, &3}

func ToSlicePtr[T any](collection []T) []*T
```

### FromSlicePtr

将指针切片转换回值切片，其中 nil 指针变为零值。

```go
a, b, c := 1, 2, 3
ptrs := []*int{&a, &b, &c}
slice := lo.FromSlicePtr(ptrs) // []int{1, 2, 3}

func FromSlicePtr[T any](collection []*T) []T
```

### Empty

返回指定类型的零值。

```go
lo.Empty[string]() // ""
lo.Empty[int]()    // 0
lo.Empty[bool]()   // false

func Empty[T any]() T
```

### IsEmpty

如果值为空（零值）则返回 true。

```go
lo.IsEmpty("")      // true
lo.IsEmpty("hello") // false
lo.IsEmpty(0)       // true

func IsEmpty[T comparable](v T) bool
```

### IsNotEmpty

IsEmpty 的反向操作。

```go
lo.IsNotEmpty("")      // false
lo.IsNotEmpty("hello") // true
lo.IsNotEmpty(42)      // true

func IsNotEmpty[T comparable](v T) bool
```

### ToAnySlice

将类型化切片转换为 `[]any`。

```go
ints := []int{1, 2, 3}
lo.ToAnySlice(ints) // []any{1, 2, 3}

func ToAnySlice[T any](collection []T) []any
```

### FromAnySlice

将 `[]any` 转换回类型化切片，附带表示是否全部断言成功的布尔值。

```go
data := []any{1, 2, 3}
result, ok := lo.FromAnySlice[int](data) // []int{1, 2, 3}, true

func FromAnySlice[T any](in []any) ([]T, bool)
```

### Coalesce

返回参数中第一个非零值，附带表示是否找到的布尔值（类似 SQL 的 COALESCE）。

```go
result, ok := lo.Coalesce("", "foo", "bar") // "foo", true
result2, ok2 := lo.Coalesce(0, 42, 100)     // 42, true

func Coalesce[T comparable](values ...T) (T, bool)
```

### CoalesceOrEmpty

返回第一个非零值，若全部为零则返回零值（Coalesce 的简化版，省去布尔返回）。

```go
lo.CoalesceOrEmpty("", "foo") // "foo"
lo.CoalesceOrEmpty("", "")    // ""

func CoalesceOrEmpty[T comparable](v ...T) T
```

### CoalesceSlice

返回第一个非空切片，附带表示是否找到的布尔值。

```go
result, ok := lo.CoalesceSlice([]int{}, []int{1, 2}) // []int{1, 2}, true

func CoalesceSlice[T any](v ...[]T) ([]T, bool)
```

### CoalesceSliceOrEmpty

返回第一个非空切片，若全部为空则返回空切片。

```go
lo.CoalesceSliceOrEmpty([]int{}, []int{1, 2}) // []int{1, 2}

func CoalesceSliceOrEmpty[T any](v ...[]T) []T
```

### CoalesceMap

返回第一个非空映射，附带表示是否找到的布尔值。

```go
result, ok := lo.CoalesceMap(map[string]int{}, map[string]int{"a": 1})
// map[string]int{"a": 1}, true

func CoalesceMap[K comparable, V any](v ...map[K]V) (map[K]V, bool)
```

### CoalesceMapOrEmpty

返回第一个非空映射，若全部为空则返回空映射。

```go
lo.CoalesceMapOrEmpty(map[string]int{}, map[string]int{"a": 1})
// map[string]int{"a": 1}

func CoalesceMapOrEmpty[K comparable, V any](v ...map[K]V) map[K]V
```

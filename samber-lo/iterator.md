# samber/lo - Iterator 迭代器包文档（lo/it）

## 概述

Iterator 包（`lo/it`）为 Go 1.23+ 提供惰性求值的序列操作工具，包含 100+ 个函数。它不进行任何缓冲，而是通过 Go 1.23 的 `iter.Seq` 和 `iter.Seq2` 实现高效的内存使用。所有操作都是链式且惰性的。

> 源文档：https://lo.samber.dev/docs/iterator
> 官方仓库：https://github.com/samber/lo
> 包导入：`github.com/samber/lo/it`

---

## Slice（切片相关）

### Range

生成一个从 0 开始的整数序列。负长度产生递减序列。

```go
import "github.com/samber/lo/it"

seq := it.Range(4)
for v := range seq {
    fmt.Println(v) // 依次输出 0, 1, 2, 3
}

func Range(elementNum int) iter.Seq[int]
```

### RangeFrom

从指定起始值开始，按定义长度生成序列。

```go
seq := it.RangeFrom(10, 3)
for v := range seq {
    fmt.Println(v) // 依次输出 10, 11, 12
}

func RangeFrom(start int, elementNum int) iter.Seq[int]
```

### RangeWithSteps

从 start 到 end（不含）按 step 步进生成序列；step 为 0 时 panic。

```go
seq := it.RangeWithSteps(0, 10, 2)
for v := range seq {
    fmt.Println(v) // 依次输出 0, 2, 4, 6, 8
}

func RangeWithSteps(start, end, step int) iter.Seq[int]
```

### Repeat

无限重复一个值的序列。

```go
seq := it.Repeat("a")
count := 0
for v := range seq {
    fmt.Println(v) // 依次输出 "a", "a", "a", ...
    count++
    if count >= 3 { break }
}

func Repeat[T any](element T) iter.Seq[T]
```

### RepeatTimes

重复一个值指定的次数。

```go
seq := it.RepeatTimes(5, "x")
for v := range seq {
    fmt.Println(v) // 依次输出 "x" 5 次
}

func RepeatTimes[T any](times int, element T) iter.Seq[T]
```

---

## Map（映射相关）

### Keys

提取映射的所有键作为序列。

```go
m := map[string]int{"foo": 1, "bar": 2}
seq := it.Keys(m)
for k := range seq {
    fmt.Println(k) // 键顺序不保证
}

func Keys[K comparable, V any](in map[K]V) iter.Seq[K]
```

### Values

提取映射的所有值作为序列。

```go
m := map[string]int{"foo": 1, "bar": 2}
seq := it.Values(m)
for v := range seq {
    fmt.Println(v) // 值顺序不保证
}

func Values[K comparable, V any](in map[K]V) iter.Seq[V]
```

### Entries

提取映射的键值对作为序列。

```go
m := map[string]int{"foo": 1, "bar": 2}
seq := it.Entries(m)
for k, v := range seq {
    fmt.Printf("%s=%d\n", k, v)
}

func Entries[K comparable, V any](in map[K]V) iter.Seq2[K, V]
```

---

## Sequence（序列变换）

### Filter

过滤序列，仅保留使谓词返回 true 的元素。

```go
seq := it.Range(10)
filtered := it.Filter(seq, func(x int) bool {
    return x%2 == 0
})
for v := range filtered {
    fmt.Println(v) // 依次输出 0, 2, 4, 6, 8
}

func Filter[T any](seq iter.Seq[T], predicate func(item T) bool) iter.Seq[T]
```

### Map

转换序列中的每个元素。

```go
seq := it.Range(3)
mapped := it.Map(seq, func(x int) string {
    return fmt.Sprintf("num_%d", x)
})
for v := range mapped {
    fmt.Println(v) // 依次输出 "num_0", "num_1", "num_2"
}

func Map[T any, R any](seq iter.Seq[T], transform func(item T) R) iter.Seq[R]
```

### FlatMap

将每个元素映射为一个序列，再展平为单个序列。

```go
seq := it.Range(3)
flattened := it.FlatMap(seq, func(x int) iter.Seq[int] {
    return it.RepeatTimes(2, x)
})
for v := range flattened {
    fmt.Println(v) // 依次输出 0, 0, 1, 1, 2, 2
}

func FlatMap[T any, R any](seq iter.Seq[T], transform func(item T) iter.Seq[R]) iter.Seq[R]
```

### Chunk

将序列分组为指定大小的块。

```go
seq := it.Range(7)
chunks := it.Chunk(seq, 3)
for chunk := range chunks {
    fmt.Println(chunk) // 依次输出 [0 1 2], [3 4 5], [6]
}

func Chunk[T any, Slice ~[]T](seq iter.Seq[T], size int) iter.Seq[Slice]
```

### Take

从序列开头取出前 N 个元素。

```go
seq := it.Range(10)
taken := it.Take(seq, 3)
for v := range taken {
    fmt.Println(v) // 依次输出 0, 1, 2
}

func Take[T any](seq iter.Seq[T], n int) iter.Seq[T]
```

### Drop

从序列开头跳过前 N 个元素。

```go
seq := it.Range(5)
dropped := it.Drop(seq, 2)
for v := range dropped {
    fmt.Println(v) // 依次输出 2, 3, 4
}

func Drop[T any](seq iter.Seq[T], n int) iter.Seq[T]
```

### TakeWhile

从序列开头取元素，直到谓词返回 false。

```go
seq := it.Range(5)
taken := it.TakeWhile(seq, func(x int) bool {
    return x < 3
})
for v := range taken {
    fmt.Println(v) // 依次输出 0, 1, 2
}

func TakeWhile[T any](seq iter.Seq[T], predicate func(item T) bool) iter.Seq[T]
```

### DropWhile

从序列开头跳过元素，直到谓词返回 false。

```go
seq := it.Range(5)
dropped := it.DropWhile(seq, func(x int) bool {
    return x < 2
})
for v := range dropped {
    fmt.Println(v) // 依次输出 2, 3, 4
}

func DropWhile[T any](seq iter.Seq[T], predicate func(item T) bool) iter.Seq[T]
```

### Uniq

去重序列，仅保留相邻不重复的元素。

```go
seq := it.SliceToSeq([]int{1, 1, 2, 2, 3, 1})
uniq := it.Uniq(seq)
for v := range uniq {
    fmt.Println(v) // 依次输出 1, 2, 3, 1
}

func Uniq[T comparable](seq iter.Seq[T]) iter.Seq[T]
```

### Concat

串联多个序列。

```go
seq1 := it.Range(2)
seq2 := it.RangeFrom(10, 2)
concat := it.Concat(seq1, seq2)
for v := range concat {
    fmt.Println(v) // 依次输出 0, 1, 10, 11
}

func Concat[T any](seqs ...iter.Seq[T]) iter.Seq[T]
```

### Zip2 / Zip3 / ...

并行迭代多个序列，返回元组序列。

```go
seq1 := it.Range(3)
seq2 := it.RepeatTimes(3, "x")
zipped := it.Zip2(seq1, seq2)
for a, b := range zipped {
    fmt.Printf("(%d, %s)\n", a, b)
    // 依次输出 (0, x), (1, x), (2, x)
}

func Zip2[A any, B any](seq1 iter.Seq[A], seq2 iter.Seq[B]) iter.Seq2[A, B]
// ... Zip3 到 Zip9 变体类似
```

### Unzip2 / Unzip3 / ...

将元组序列拆分为多个序列。

```go
seq := it.Zip2(it.Range(3), it.RepeatTimes(3, "x"))
a, b := it.Unzip2(seq)
for v := range a {
    fmt.Println(v) // 依次输出 0, 1, 2
}

func Unzip2[A any, B any](seq iter.Seq2[A, B]) (iter.Seq[A], iter.Seq[B])
// ... Unzip3 到 Unzip9 变体类似
```

---

## Find（查找相关）

### IndexOf

返回序列中第一个等于给定值的元素的索引，未找到返回 -1。

```go
seq := it.SliceToSeq([]int{10, 20, 30, 20})
idx := it.IndexOf(seq, 20) // 1

func IndexOf[T comparable](seq iter.Seq[T], element T) int
```

### LastIndexOf

返回序列中最后一个等于给定值的元素的索引，未找到返回 -1。

```go
seq := it.SliceToSeq([]int{10, 20, 30, 20})
idx := it.LastIndexOf(seq, 20) // 3

func LastIndexOf[T comparable](seq iter.Seq[T], element T) int
```

### Find

搜索序列中第一个满足谓词的元素。

```go
seq := it.Range(10)
val, ok := it.Find(seq, func(x int) bool {
    return x > 5
}) // 6, true

func Find[T any](seq iter.Seq[T], predicate func(item T) bool) (T, bool)
```

### FindIndex

搜索序列中第一个满足谓词的元素，返回元素和索引。

```go
seq := it.Range(10)
val, idx, ok := it.FindIndex(seq, func(x int) bool {
    return x > 5
}) // 6, 6, true

func FindIndex[T any](seq iter.Seq[T], predicate func(item T) bool) (T, int, bool)
```

### Contains

判断序列中是否存在给定元素。

```go
seq := it.Range(5)
ok := it.Contains(seq, 3) // true

func Contains[T comparable](seq iter.Seq[T], element T) bool
```

---

## Intersect（交集相关）

### Every

判断是否所有元素都满足谓词。

```go
seq := it.Range(5)
ok := it.Every(seq, func(x int) bool {
    return x < 10
}) // true

func Every[T any](seq iter.Seq[T], predicate func(item T) bool) bool
```

### Some

判断是否至少有一个元素满足谓词。

```go
seq := it.Range(5)
ok := it.Some(seq, func(x int) bool {
    return x > 3
}) // true

func Some[T any](seq iter.Seq[T], predicate func(item T) bool) bool
```

### None

判断是否没有元素满足谓词。

```go
seq := it.Range(5)
ok := it.None(seq, func(x int) bool {
    return x < 0
}) // true

func None[T any](seq iter.Seq[T], predicate func(item T) bool) bool
```

---

## Channel（通道相关）

### SliceToSeq

将切片转换为序列。

```go
slice := []int{1, 2, 3}
seq := it.SliceToSeq(slice)
for v := range seq {
    fmt.Println(v) // 依次输出 1, 2, 3
}

func SliceToSeq[T any](collection []T) iter.Seq[T]
```

### SliceToSeq2

将切片转换为二元序列（键值对）。

```go
slice := []int{10, 20, 30}
seq := it.SliceToSeq2(slice)
for i, v := range seq {
    fmt.Printf("[%d]=%d\n", i, v) // 依次输出 [0]=10, [1]=20, [2]=30
}

func SliceToSeq2[T any](collection []T) iter.Seq2[int, T]
```

---

## Aggregation（聚合相关）

### Count

计算序列中满足谓词的元素数量。

```go
seq := it.Range(10)
count := it.Count(seq, func(x int) bool {
    return x%2 == 0
}) // 5

func Count[T any](seq iter.Seq[T], predicate func(item T) bool) int
```

### Sum

计算序列中所有元素的和。

```go
seq := it.Range(5)
total := it.Sum(seq) // 0+1+2+3+4 = 10

func Sum[T constraints.Ordered](seq iter.Seq[T]) T
```

### GroupBy

按键函数对序列元素分组，返回映射序列。

```go
seq := it.Range(6)
groups := it.GroupBy(seq, func(x int) string {
    if x%2 == 0 { return "even" }
    return "odd"
})
for key := range groups {
    fmt.Println(key) // 返回 "even", "odd"
}

func GroupBy[T any, K comparable](seq iter.Seq[T], iteratee func(item T) K) iter.Seq2[K, iter.Seq[T]]
```

### ForEach

遍历序列中的每个元素并执行回调。

```go
seq := it.Range(3)
it.ForEach(seq, func(x int) {
    fmt.Println(x) // 依次输出 0, 1, 2
})

func ForEach[T any](seq iter.Seq[T], callback func(item T))
```

### Reduce

将序列归约为单个值。

```go
seq := it.Range(1, 5)
sum := it.Reduce(seq, func(agg int, item int) int {
    return agg + item
}, 0) // 1+2+3+4 = 10

func Reduce[T any, R any](seq iter.Seq[T], accumulator func(agg R, item T) R, initial R) R
```

---

## 常用组合示例

**过滤 + 转换 + 聚合链式操作**

```go
seq := it.Range(10)
result := it.Sum(
    it.Map(
        it.Filter(seq, func(x int) bool { return x%2 == 0 }),
        func(x int) int { return x * 2 },
    ),
) // (0*2 + 2*2 + 4*2 + 6*2 + 8*2) = 40

func main() {
    // Iterator 的惰性求值：直到遍历时才计算
}
```

**重要特性**

- **惰性求值**：序列直到被遍历（range）时才计算，不会一次性加载所有数据。
- **零缓冲**：不进行中间集合存储，内存高效。
- **链式组合**：可任意组合多个操作（Filter、Map、FlatMap 等）。
- **Go 1.23+**：需要 Go 1.23 或更高版本以支持 `iter.Seq` 类型。

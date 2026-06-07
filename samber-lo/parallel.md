# samber/lo - Parallel 并行包文档（lo/parallel）

## 概述

Parallel 包（`lo/parallel`）提供利用 Go 协程进行并发处理的集合操作工具。它内置工作池（默认 4 个工作线程）来管理并发，适用于 CPU 密集型和 I/O 密集型任务。所有操作都是自动并行化的，无需手动管理 goroutine。

> 源文档：https://lo.samber.dev/docs/parallel
> 官方仓库：https://github.com/samber/lo
> 包导入：`github.com/samber/lo/parallel`

---

## Slice（切片相关）

### Map

并行转换切片中的每个元素为另一类型。内部使用工作池分派任务。

```go
import (
    "strconv"
    lop "github.com/samber/lo/parallel"
)

out := lop.Map([]int64{1, 2, 3, 4}, func(x int64, i int) string {
    return strconv.FormatInt(x, 10)
})
// []string{"1", "2", "3", "4"}

func Map[T any, R any](collection []T, transform func(item T, index int) R) []R
```

### ForEach

并行迭代切片中的每个元素并执行回调。不保证执行顺序。

```go
import (
    "fmt"
    lop "github.com/samber/lo/parallel"
)

lop.ForEach([]string{"hello", "world"}, func(x string, _ int) {
    fmt.Println(x)
})
// 输出顺序不确定（取决于调度）

func ForEach[T any](collection []T, callback func(item T, index int))
```

### Times

并行调用谓词函数 count 次，并收集结果。

```go
import (
    "strconv"
    lop "github.com/samber/lo/parallel"
)

nums := lop.Times(5, func(i int) string {
    return strconv.Itoa(i)
})
// []string{"0", "1", "2", "3", "4"}
// 5 个任务可能被并行执行

func Times[T any](count int, iteratee func(index int) T) []T
```

### GroupBy

并行按键函数对元素分组，返回映射。各分组的计算是并行的。

```go
import lop "github.com/samber/lo/parallel"

groups := lop.GroupBy([]int{0, 1, 2, 3, 4, 5}, func(i int) int {
    return i % 3
})
// map[int][]int{
//     0: {0, 3},
//     1: {1, 4},
//     2: {2, 5},
// }

func GroupBy[T any, U comparable, Slice ~[]T](collection Slice, iteratee func(item T) U) map[U]Slice
```

### PartitionBy

并行按键函数对元素分组，保留相邻元素的分组关系。

```go
import lop "github.com/samber/lo/parallel"

groups := lop.PartitionBy([]int{-2, -1, 0, 1, 2, 3, 4, 5}, func(x int) string {
    if x < 0 { return "negative" }
    if x%2 == 0 { return "even" }
    return "odd"
})
// [][]int{{-2, -1}, {0, 2, 4}, {1, 3, 5}}

func PartitionBy[T any, K comparable, Slice ~[]T](collection Slice, iteratee func(item T) K) []Slice
```

---

## Filter 系列

### Filter

并行过滤切片。返回所有使谓词返回 true 的元素。

```go
import lop "github.com/samber/lo/parallel"

result := lop.Filter([]int{1, 2, 3, 4, 5}, func(x int) bool {
    return x%2 == 0
})
// []int{2, 4}

func Filter[T any, Slice ~[]T](collection Slice, predicate func(item T) bool) Slice
```

### Reject

并行拒绝切片。返回所有使谓词返回 false 的元素（Filter 的反向操作）。

```go
result := lop.Reject([]int{1, 2, 3, 4, 5}, func(x int) bool {
    return x%2 == 0
})
// []int{1, 3, 5}

func Reject[T any, Slice ~[]T](collection Slice, predicate func(item T) bool) Slice
```

---

## Find 系列

### Find

并行搜索第一个满足谓词的元素。返回元素和布尔值。

```go
result, ok := lop.Find([]int{1, 2, 3, 4, 5}, func(x int) bool {
    return x > 3
})
// 4 或 5（取决于并行顺序）, true

func Find[T any](collection []T, predicate func(item T) bool) (T, bool)
```

### FindIndex

并行搜索第一个满足谓词的元素，返回元素、索引和布尔值。

```go
result, idx, ok := lop.FindIndex([]int{10, 20, 30}, func(x int) bool {
    return x > 15
})
// 可能返回 (20, 1, true) 或 (30, 2, true)

func FindIndex[T any](collection []T, predicate func(item T) bool) (T, int, bool)
```

### Contains

并行判断切片中是否存在给定元素。

```go
ok := lop.Contains([]int{1, 2, 3, 4}, 3)
// true

func Contains[T comparable](collection []T, element T) bool
```

---

## Intersect 系列

### Every

并行判断是否所有元素都满足谓词。

```go
ok := lop.Every([]int{2, 4, 6}, func(x int) bool {
    return x%2 == 0
})
// true

func Every[T any](collection []T, predicate func(item T) bool) bool
```

### Some

并行判断是否至少有一个元素满足谓词。

```go
ok := lop.Some([]int{1, 2, 3, 4}, func(x int) bool {
    return x > 3
})
// true

func Some[T any](collection []T, predicate func(item T) bool) bool
```

### None

并行判断是否没有元素满足谓词。

```go
ok := lop.None([]int{1, 2, 3}, func(x int) bool {
    return x > 10
})
// true

func None[T any](collection []T, predicate func(item T) bool) bool
```

---

## 工作池配置

### 工作线程数

Parallel 包默认使用 **4 个工作线程**。可通过环境变量或运行时调整：

```go
// 通过修改源码配置（默认在 parallel/stream.go 中）
// 或者通过编译时标志调整
```

**何时增加工作线程数**

- I/O 密集型操作（网络请求、文件读写）
- 机器有多个 CPU 核心且任务足够多

**何时减少工作线程数**

- CPU 密集型操作
- 为其他系统任务留出资源

### 任务调度

所有 Parallel 函数内部使用 channel 和 goroutine 管理任务分发：

```go
// 概念性示例（实际实现更复杂）
workers := 4
tasks := make(chan Task, 100)
results := make(chan Result)

// 启动 workers 个 worker goroutine
for i := 0; i < workers; i++ {
    go worker(tasks, results)
}

// 分发任务
for _, item := range collection {
    tasks <- Task{item: item, fn: transformFn}
}
close(tasks)

// 收集结果
for i := 0; i < len(collection); i++ {
    results := <-results
}
```

---

## 性能考量

**何时使用 Parallel**

- 集合包含数百个以上元素
- 每个元素的处理时间 ≥ 1ms（足以抵消并发开销）
- I/O 密集型操作（等待网络、文件）

**何时避免 Parallel**

- 集合很小（< 100 元素），并发开销会抵消收益
- CPU 密集型任务且集合大小 > 核心数
- 需要确定的执行顺序

**并发安全**

Parallel 函数是并发安全的（使用回调函数的除外）。如果回调函数修改全局状态，需要自行同步。

```go
// ⚠️ 不安全
var sum int
lop.ForEach(items, func(x int, _ int) {
    sum += x // 竞态条件！
})

// ✅ 安全：使用 Reduce 或 Map 收集结果
results := lop.Map(items, func(x int, _ int) int {
    return x * 2
})
```

---

## 实际用例

**并行批量网络请求**

```go
urls := []string{"http://a.com", "http://b.com", "http://c.com"}
results := lop.Map(urls, func(url string, _ int) (string, error) {
    resp, err := http.Get(url)
    if err != nil { return "", err }
    defer resp.Body.Close()
    body, _ := ioutil.ReadAll(resp.Body)
    return string(body), nil
})
// 3 个请求并行执行
```

**并行数据处理管道**

```go
data := []int{1, 2, 3, 4, 5}
filtered := lop.Filter(data, func(x int) bool {
    return x%2 == 0
})
transformed := lop.Map(filtered, func(x int, _ int) string {
    return fmt.Sprintf("num_%d", x)
})
// 过滤和映射并行执行
```

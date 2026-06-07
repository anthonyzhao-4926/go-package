# samber/lo - Go 泛型工具库完整文档

## 🚀 快速开始

**samber/lo** 是一个为 Go 1.18+ 设计的 Lodash 风格工具集合，采用泛型实现类型安全的操作。它包含 **500+ 个工具函数**，组织在 4 个专门的包中。

### 安装

```bash
go get -u github.com/samber/lo@v1
```

### 导入

```go
import "github.com/samber/lo"                // Core 包
import "github.com/samber/lo/it"             // Iterator 包（Go 1.23+）
import lom "github.com/samber/lo/mutable"    // Mutable 包
import lop "github.com/samber/lo/parallel"   // Parallel 包
```

### 基本示例

```go
import "github.com/samber/lo"

// 映射：将每个元素平方
numbers := []int{1, 2, 3, 4, 5}
squared := lo.Map(numbers, func(x int, _ int) int {
    return x * x
})
// 结果: [1, 4, 9, 16, 25]

// 过滤：保留偶数
evens := lo.Filter(numbers, func(x int, _ int) bool {
    return x%2 == 0
})
// 结果: [2, 4]

// 分组：按模 3 分组
groups := lo.GroupBy(numbers, func(x int) int {
    return x % 3
})
// 结果: map[int][]int{0: {3}, 1: {1, 4}, 2: {2, 5}}
```

---

## 📚 文档文件

本项目收集了 **samber/lo** 官方文档的完整中文翻译和补充示例。

| 文件 | 说明 | 函数数 | 特点 |
|------|------|--------|------|
| **core.md** | 核心模块（`lo`）| 203 | 完整结果、函数式、Go 1.18+ |
| **iterator.md** | 迭代器包（`lo/it`） | 35+ | 惰性求值、零缓冲、Go 1.23+ |
| **mutable.md** | 可变包（`lo/mutable`） | 6 | 原地修改、性能优先 |
| **parallel.md** | 并行包（`lo/parallel`） | 15+ | 并发处理、多核加速、自动分派 |

---

## 🔍 四大包对比速查表

| 维度 | Core (`lo`) | Iterator (`lo/it`) | Mutable (`lo/mutable`) | Parallel (`lo/parallel`) |
|------|-------------|------------------|----------------------|------------------------|
| **内存** | 每次操作分配新切片 | 零缓冲，按需生成 | 原地修改，无额外分配 | 自动分派，与 Core 相同 |
| **性能** | 中等（创建切片开销） | 最优（提前中断节省） | 最优（无分配） | 加速 ~4x（4核并行） |
| **适用场景** | 一般用途、安全优先 | 大集合、流式处理 | 性能关键、临时数据 | CPU/IO 密集、大集合 |
| **Go 版本** | 1.18+ | 1.23+ | 1.18+ | 1.18+ |
| **结果顺序** | 确定 | 确定 | 确定 | 不确定 |
| **副作用** | 无（安全） | 无 | 有（修改原数据） | 无 |
| **文档** | 203 函数，全覆盖 | 35+ 函数，惰性说明 | 6 函数，性能对比 | 15+ 函数，并发说明 |

---

**官方仓库**：https://github.com/samber/lo  
**官方文档**：https://lo.samber.dev/  
**最后更新**：2026-06-07 | **总函数数**：500+ | **总文档行数**：3872

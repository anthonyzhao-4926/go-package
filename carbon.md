# Carbon 使用方法快捷查询

## 项目简介

`Carbon`  是一个轻量级、语义化、对开发者友好的 `golang` 时间处理库，不依赖于 `任何` 第三方库， `100%` 单元测试覆盖率，已被 [docker](https://github.com/docker/docker-language-server/blob/main/go.mod#L1 "docker") 组织使用以及被 [awesome-go](https://github.com/yinggaozhen/awesome-go-cn#日期和时间 "awesome-go-cn") 和 [hello-github](https://hellogithub.com/repository/dromara/carbon "hello-github") 收录，并获得
`gitee` 2024 年最有价值项目（`GVP`）和 `gitcode` 2024 年度开源摘星计划 (`G-Star`) 项目

### 项目特性
- 轻量级 & 零依赖：除单元测试外，不依赖`任何`第三方库，`100%` 单元测试覆盖率，保证代码质量和稳定性
- 语义化API：提提供语义化、对开发者友好的 API，简洁、优雅的链式调用，保证代码可扩展性和可重用性
- 强大的时间操作：支持时间解析、时间加减、时间设置、时间边界、时间判断、时间差值、时间极值等
- 丰富的获取方式：获取时间的各个部分（如年、月、日、时、分、秒等）以及不同精度的时间戳
- 多样化的输出格式：按需输出不同精度和格式的时间字符串，包括 ISO、RFC 系列以及自定义格式
- 测试友好：支持设置测试时间，冻结当前时间，便于单元测试
- 时区处理：支持设置和获取时区，以及不同时区之间的相互转换
- 国际化：支持 30+ 种本地化翻译语言，并允许自定义翻译资源
- 错误处理：提供错误检查机制，便于处理时间解析等错误

### 仓库地址

[github.com/dromara/carbon](https://github.com/dromara/carbon "github.com/dromara/carbon")

[gitee.com/dromara/carbon](https://gitee.com/dromara/carbon "gitee.com/dromara/carbon")

[gitcode.com/dromara/carbon](https://gitcode.com/dromara/carbon "gitcode.com/dromara/carbon")


---

## 快速开始
> go version >= 1.18

```go
// 使用 github 库
go get -u github.com/dromara/carbon/v2
import "github.com/dromara/carbon/v2"

// 使用 gitee 库
go get -u gitee.com/dromara/carbon/v2
import "gitee.com/dromara/carbon/v2"

// 使用 gitcode 库
go get -u gitcode.com/dromara/carbon/v2
import "gitcode.com/dromara/carbon/v2"
```

`carbon` 已经捐赠给了 [dromara](https://dromara.org/ "dromara") 开源组织，仓库地址发生了改变，如果之前用的路径是
`golang-module/carbon`，请在 `go.mod` 里将原地址更换为新路径，或执行如下命令

```go
go mod edit -replace github.com/golang-module/carbon/v2 = github.com/dromara/carbon/v2
```


---

## 创建 `Carbon` 实例

### 从时间戳创建 `Carbon` 实例
```go
// 从秒精度时间戳创建 Carbon 实例
carbon.CreateFromTimestamp(-1).ToString() // 1969-12-31 23:59:59 +0000 UTC
carbon.CreateFromTimestamp(0).ToString() // 1970-01-01 00:00:00 +0000 UTC
carbon.CreateFromTimestamp(1).ToString() // 1970-01-01 00:00:01 +0000 UTC
carbon.CreateFromTimestamp(1596633255).ToString() // 2020-08-05 13:14:15 +0000 UTC
// 从毫秒精度时间戳创建 Carbon 实例
carbon.CreateFromTimestampMilli(1596604455999).ToString() // 2020-08-05 13:14:15.999 +0000 UTC
// 从微秒精度时间戳创建 Carbon 实例
carbon.CreateFromTimestampMicro(1596604455999999).ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
// 从纳秒精度时间戳创建 Carbon 实例
carbon.CreateFromTimestampNano(1596604455999999999).ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC
```

### 从日期创建 `Carbon` 实例
```go
// 从年、月、日、时、分、秒创建 Carbon 实例
carbon.CreateFromDateTime(2020, 8, 5, 13, 14, 15).ToString() // 2020-08-05 13:14:15 +0000 UTC
// 从年、月、日、时、分、秒、毫秒创建 Carbon 实例
carbon.CreateFromDateTimeMilli(2020, 8, 5, 13, 14, 15, 999).ToString() // 2020-08-05 13:14:15.999 +0000 UTC
// 从年、月、日、时、分、秒、微秒创建 Carbon 实例
carbon.CreateFromDateTimeMicro(2020, 8, 5, 13, 14, 15, 999999).ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
// 从年、月、日、时、分、秒、纳秒创建 Carbon 实例
carbon.CreateFromDateTimeNano(2020, 8, 5, 13, 14, 15, 999999999).ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC

// 从年、月、日创建 Carbon 实例
carbon.CreateFromDate(2020, 8, 5).ToString() // 2020-08-05 00:00:00 +0000 UTC
// 从年、月、日、毫秒创建 Carbon 实例
carbon.CreateFromDateMilli(2020, 8, 5, 999).ToString() // 2020-08-05 00:00:00.999 +0000 UTC
// 从年、月、日、微秒创建 Carbon 实例
carbon.CreateFromDateMicro(2020, 8, 5, 999999).ToString() // 2020-08-05 00:00:00.999999 +0000 UTC
// 从年、月、日、纳秒创建 Carbon 实例
carbon.CreateFromDateNano(2020, 8, 5, 999999999).ToString() // 2020-08-05 00:00:00.999999999 +0000 UTC

// 从时、分、秒创建 Carbon 实例(年月日默认为当前年月日)
carbon.CreateFromTime(13, 14, 15).ToString() // 2020-08-05 13:14:15 +0000 UTC
// 从时、分、秒、毫秒创建 Carbon 实例(年月日默认为当前年月日)
carbon.CreateFromTimeMilli(13, 14, 15, 999).ToString() // 2020-08-05 13:14:15.999 +0000 UTC
// 从时、分、秒、微秒创建 Carbon 实例(年月日默认为当前年月日)
carbon.CreateFromTimeMicro(13, 14, 15, 999999).ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
// 从时、分、秒、纳秒创建 Carbon 实例(年月日默认为当前年月日)
carbon.CreateFromTimeNano(13, 14, 15, 999999999).ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC
```


---

## `carbon` 、 `time.Time` 之间互转

### 将标准 `time.Time` 转换成 `carbon`

```go
carbon.NewCarbon(time.Now())
carbon.NewCarbon(time.Now().In(time.Local))
```
或
```go
carbon.CreateFromStdTime(time.Now())
carbon.CreateFromStdTime(time.Now().In(time.Local))
```

### 将 `carbon` 转换成标准 `time.Time`

```go
carbon.Now().StdTime()
carbon.Now(carbon.Local).StdTime()
```


---

## 时间解析
该系列方法不支持 `时间戳` 字符串解析，解析时间戳请使用 `CreateFromTimestamp`, `CreateFromTimestampMilli`, `CreateFromTimestampMicro`, `CreateFromTimestampNano` 方法

### 通过默认 `布局模板` 解析
通过默认 `布局模板` 将时间字符串解析成 `Carbon` 实例
```go
carbon.Parse("").ToDateTimeString() // 空字符串
carbon.Parse("00:00:00").ToDateTimeString() // 空字符串
carbon.Parse("0000-00-00").ToDateTimeString() // 空字符串
carbon.Parse("0000-00-00 00:00:00").ToDateTimeString() // 空字符串

carbon.Parse("now").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("yesterday").ToString() // 2020-08-04 13:14:15 +0000 UTC
carbon.Parse("tomorrow").ToString() // 2020-08-06 13:14:15 +0000 UTC

carbon.Parse("2020").ToString() // 2020-01-01 00:00:00 +0000 UTC
carbon.Parse("2020-8").ToString() // 2020-08-01 00:00:00 +0000 UTC
carbon.Parse("2020-08").ToString() // 2020-08-01 00:00:00 +0000 UTC
carbon.Parse("2020-8-5").ToString() // 2020-08-05 00:00:00 +0000 UTC
carbon.Parse("2020-8-05").ToString() // 2020-08-05 00:00:00 +0000 UTC
carbon.Parse("2020-08-05").ToString() // 2020-08-05 00:00:00 +0000 UTC
carbon.Parse("2020-08-05.999").ToString() // 2020-08-05 00:00:00.999 +0000 UTC
carbon.Parse("2020-08-05.999999").ToString() // 2020-08-05 00:00:00.999999 +0000 UTC
carbon.Parse("2020-08-05.999999999").ToString() // 2020-08-05 00:00:00.999999999 +0000 UTC

carbon.Parse("2020-8-5 13:14:15").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-8-05 13:14:15").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-08-5 13:14:15").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-08-05 13:14:15").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-08-05 13:14:15.999").ToString() // 2020-08-05 13:14:15.999 +0000 UTC
carbon.Parse("2020-08-05 13:14:15.999999").ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
carbon.Parse("2020-08-05 13:14:15.999999999").ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC

carbon.Parse("2020-8-5T13:14:15+08:00").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-8-05T13:14:15+08:00").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-08-05T13:14:15+08:00").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-08-05T13:14:15.999+08:00").ToString() // 2020-08-05 13:14:15.999 +0000 UTC
carbon.Parse("2020-08-05T13:14:15.999999+08:00").ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC

carbon.Parse("20200805").ToString() // 2020-08-05 00:00:00 +0000 UTC
carbon.Parse("20200805131415").ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("20200805131415.999").ToString() // 2020-08-05 13:14:15.999 +0000 UTC
carbon.Parse("20200805131415.999999").ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
carbon.Parse("20200805131415.999999999").ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC
carbon.Parse("20200805131415.999+08:00").ToString() // 2020-08-05 13:14:15.999 +0000 UTC
carbon.Parse("20200805131415.999999+08:00").ToString() // 2020-08-05 13:14:15.999999 +0000 UTC
carbon.Parse("20200805131415.999999999+08:00").ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC

carbon.Parse("2022-03-08T03:01:14-07:00").ToString() // 2022-03-08 18:01:14 +0000 UTC
carbon.Parse("2022-03-08T10:01:14Z").ToString() // 2022-03-08 18:01:14 +0000 UTC
```

### 通过一个 `布局模板` 解析
通过一个确认的 `布局模板` 将时间字符串解析成 `Carbon` 实例
```go
carbon.ParseByLayout("2020|08|05 13|14|15", "2006|01|02 15|04|05").ToDateTimeString() // 2020-08-05 13:14:15
carbon.ParseByLayout("It is 2020-08-05 13:14:15", "It is 2006-01-02 15:04:05").ToDateTimeString() // 2020-08-05 13:14:15
carbon.ParseByLayout("今天是 2020年08月05日13时14分15秒", "今天是 2006年01月02日15时04分05秒").ToDateTimeString() // 2020-08-05 13:14:15
carbon.ParseByLayout("2020-08-05 13:14:15", "2006-01-02 15:04:05", carbon.Tokyo).ToDateTimeString() // 2020-08-05 14:14:15
```

### 通过一个 `格式模板` 解析
通过一个确认 `格式模板` 将时间字符串解析成 `Carbon` 实例
> 注：如果使用的字母与格式模板冲突时，请使用转义符 "\\" 转义该字母
```go
carbon.ParseByFormat("2020|08|05 13|14|15", "Y|m|d H|i|s").ToDateTimeString() // 2020-08-05 13:14:15
carbon.ParseByFormat("It is 2020-08-05 13:14:15", "\\I\\t \\i\\s Y-m-d H:i:s").ToDateTimeString() // 2020-08-05 13:14:15
carbon.ParseByFormat("今天是 2020年08月05日13时14分15秒", "今天是 Y年m月d日H时i分s秒").ToDateTimeString() // 2020-08-05 13:14:15
carbon.ParseByFormat("2020-08-05 13:14:15", "Y-m-d H:i:s", carbon.Tokyo).ToDateTimeString() // 2020-08-05 14:14:15
```

### 通过多个 `布局模板` 解析
通过多个模糊的 `布局模板` 将时间字符串解析成 `Carbon` 实例
```go
c := carbon.ParseByLayouts("2020|08|05 13|14|15", []string{"2006|01|02 15|04|05", "2006|1|2 3|4|5"})
c.ToDateTimeString() // 2020-08-05 13:14:15
c.CurrentLayout() // 2006|01|02 15|04|05
```

### 通过多个 `格式模板` 解析
通过多个模糊的 `格式模板` 将时间字符串解析成 `Carbon` 实例
```go
c := carbon.ParseByFormats("2020|08|05 13|14|15", []string{"Y|m|d H|i|s", "y|m|d h|i|s"})
c.ToDateTimeString() // 2020-08-05 13:14:15
c.CurrentLayout() // 2006|01|02 15|04|05
```


---

## 时间输出

```go
// 输出日期时间字符串
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToDateTimeString() // 2020-08-05 13:14:15
// 输出日期时间字符串，包含毫秒
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToDateTimeMilliString() // 2020-08-05 13:14:15.999
// 输出日期时间字符串，包含微秒
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToDateTimeMicroString() // 2020-08-05 13:14:15.999999
// 输出日期时间字符串，包含纳秒
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToDateTimeNanoString() // 2020-08-05 13:14:15.999999999

// 输出简写日期时间字符串
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToShortDateTimeString() // 20200805131415
// 输出简写日期时间字符串，包含毫秒
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToShortDateTimeMilliString() // 20200805131415.999
// 输出简写日期时间字符串，包含微秒
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToShortDateTimeMicroString() // 20200805131415.999999
// 输出简写日期时间字符串，包含纳秒
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToShortDateTimeNanoString() // 20200805131415.999999999

// 输出日期字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToDateString() // 2020-08-05
// 输出日期字符串，包含毫秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToDateMilliString() // 2020-08-05.999
// 输出日期字符串，包含微秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToDateMicroString() // 2020-08-05.999999
// 输出日期字符串，包含纳秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToDateNanoString() // 2020-08-05.999999999

// 输出简写日期字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortDateString() // 20200805
// 输出简写日期字符串，包含毫秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortDateMilliString() // 20200805.999
// 输出简写日期字符串，包含微秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortDateMicroString() // 20200805.999999
// 输出简写日期字符串，包含纳秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortDateNanoString() // 20200805.999999999

// 输出时间字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToTimeString() // 13:14:15
// 输出时间字符串，包含毫秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToTimeMilliString() // 13:14:15.999
// 输出时间字符串，包含微秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToTimeMicroString() // 13:14:15.999999
// 输出时间字符串，包含纳秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToTimeNanoString() // 13:14:15.999999999

// 输出简写时间字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortTimeString() // 131415
// 输出简写时间字符串，包含毫秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortTimeMilliString() // 131415.999
// 输出简写时间字符串，包含微秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortTimeMicroString() // 131415.999999
// 输出简写时间字符串，包含纳秒
carbon.Parse("2020-08-05 13:14:15.999999999").ToShortTimeNanoString() // 131415.999999999

// 输出 Ansic 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToAnsicString() // Wed Aug  5 13:14:15 2020
// 输出 Atom 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToAtomString() // 2020-08-05T13:14:15+00:00
// 输出 UnixDate 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToUnixDateString() // Wed Aug  5 13:14:15 UTC 2020
// 输出 RubyDate 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRubyDateString() // Wed Aug 05 13:14:15 +0000 2020
// 输出 Kitchen 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToKitchenString() // 1:14PM
// 输出 Cookie 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToCookieString() // Wednesday, 05-Aug-2020 13:14:15 UTC
// 输出 DayDateTime 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToDayDateTimeString() // Wed, Aug 5, 2020 1:14 PM
// 输出 RSS 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRssString() // Wed, 05 Aug 2020 13:14:15 +0000
// 输出 W3C 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToW3cString() // 2020-08-05T13:14:15+00:00
// 输出 Http 格式字符串, 如 Last-Modified 格式
carbon.Parse("2020-08-05 13:14:15").ToHttpString() // Wed, 05 Aug 2020 13:14:15 GMT

// 输出 ISO8601 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601String() // 2020-08-05T13:14:15+00:00
// 输出 ISO8601Milli 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601MilliString() // 2020-08-05T13:14:15.999+00:00
// 输出 ISO8601Micro 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601MicroString() // 2020-08-05T13:14:15.999999+00:00
// 输出 ISO8601Nano 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601NanoString() // 2020-08-05T13:14:15.999999999+00:00
// 输出 ISO8601Zulu 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601ZuluString() // 2020-08-05T13:14:15Z
// 输出 ISO8601ZuluMilli 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601ZuluMilliString() // 2020-08-05T13:14:15.999Z
// 输出 ISO8601ZuluMicro 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601ZuluMicroString() // 2020-08-05T13:14:15.999999Z
// 输出 ISO8601ZuluNano 格式字符串
carbon.Parse("2020-08-05 13:14:15.999999999").ToIso8601ZuluNanoString() // 2020-08-05T13:14:15.999999999Z

// 输出 RFC822 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc822String() // 05 Aug 20 13:14 UTC
// 输出 RFC822Z 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc822zString() // 05 Aug 20 13:14 +0000
// 输出 RFC850 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc850String() // Wednesday, 05-Aug-20 13:14:15 UTC
// 输出 RFC1036 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc1036String() // Wed, 05 Aug 20 13:14:15 +0000
// 输出 RFC1123 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc1123String() // Wed, 05 Aug 2020 13:14:15 UTC
// 输出 RFC1123Z 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc1123zString() // Wed, 05 Aug 2020 13:14:15 +0000
// 输出 RFC2822 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc2822String() // Wed, 05 Aug 2020 13:14:15 +0000
// 输出 RFC7231 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToRfc7231String() // Wed, 05 Aug 2020 13:14:15 UTC

// 输出 RFC3339 格式字符串
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToRfc3339String() // 2020-08-05T13:14:15+00:00
// 输出 RFC3339Milli 格式字符串
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToRfc3339MilliString() // 2020-08-05T13:14:15.999+00:00
// 输出 RFC3339Micro 格式字符串
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToRfc3339MicroString() // 2020-08-05T13:14:15.999999+00:00
// 输出 RFC3339Nano 格式字符串
carbon.Parse("2020-08-05T13:14:15.999999999+08:00").ToRfc3339NanoString() // 2020-08-05T13:14:15.999999999+00:00

// 输出日期时间字符串
fmt.Printf("%s", carbon.Parse("2020-08-05 13:14:15")) // 2020-08-05 13:14:15

// 输出"2006-01-02 15:04:05.999999999 -0700 MST"格式字符串
carbon.Parse("2020-08-05 13:14:15").ToString() // 2020-08-05 13:14:15.999999 +0000 UTC

// 输出 "Jan 2, 2006" 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToFormattedDateString() // Aug 5, 2020
// 输出 "Mon, Jan 2, 2006" 格式字符串
carbon.Parse("2020-08-05 13:14:15").ToFormattedDayDateString() // Wed, Aug 5, 2020

// 输出指定布局的字符串
carbon.Parse("2020-08-05 13:14:15").Layout(carbon.ISO8601Layout) // 2020-08-05T13:14:15+00:00
carbon.Parse("2020-08-05 13:14:15").Layout("20060102150405") // 20200805131415
carbon.Parse("2020-08-05 13:14:15").Layout("2006年01月02日 15时04分05秒") // 2020年08月05日 13时14分15秒
carbon.Parse("2020-08-05 13:14:15").Layout("It is 2006-01-02 15:04:05") // It is 2020-08-05 13:14:15

// 输出指定格式的字符串(如果使用的字母与格式符号冲突时，请使用\符号转义该字符)
carbon.Parse("2020-08-05 13:14:15").Format("YmdHis") // 20200805131415
carbon.Parse("2020-08-05 13:14:15").Format("Y年m月d日 H时i分s秒") // 2020年08月05日 13时14分15秒
carbon.Parse("2020-08-05 13:14:15").Format("l jK \\o\\f F Y h:i:s A") // Wednesday 5th of August 2020 01:14:15 PM
carbon.Parse("2020-08-05 13:14:15").Format("\\I\\t \\i\\s Y-m-d H:i:s") // It is 2020-08-05 13:14:15
```

更多格式输出符号请查看 <a href="/zh/appendix/format-signs.html" target="_blank" rel="noreferrer">格式化符号</a>


---

## 时间获取

```go
// 获取本年总天数
carbon.Parse("2019-08-05 13:14:15").DaysInYear() // 365
carbon.Parse("2020-08-05 13:14:15").DaysInYear() // 366
// 获取本月总天数
carbon.Parse("2020-02-01 13:14:15").DaysInMonth() // 29
carbon.Parse("2020-04-01 13:14:15").DaysInMonth() // 30
carbon.Parse("2020-08-01 13:14:15").DaysInMonth() // 31

// 获取本年第几天
carbon.Parse("2020-08-05 13:14:15").DayOfYear() // 218
// 获取本年第几周
carbon.Parse("2019-12-31 13:14:15").WeekOfYear() // 1
carbon.Parse("2020-08-05 13:14:15").WeekOfYear() // 32
// 获取本月第几天
carbon.Parse("2020-08-05 13:14:15").DayOfMonth() // 5
// 获取本月第几周
carbon.Parse("2020-08-05 13:14:15").WeekOfMonth() // 1
// 获取本周第几天
carbon.Parse("2020-08-05 13:14:15").DayOfWeek() // 3

// 获取当前年月日时分秒
carbon.Parse("2020-08-05 13:14:15").DateTime() // 2020,8,5,13,14,15
// 获取当前年月日时分秒毫秒
carbon.Parse("2020-08-05 13:14:15").DateTimeMilli() // 2020,8,5,13,14,15,999
// 获取当前年月日时分秒微秒
carbon.Parse("2020-08-05 13:14:15").DateTimeMicro() // 2020,8,5,13,14,15,999999
// 获取当前年月日时分秒纳秒
carbon.Parse("2020-08-05 13:14:15").DateTimeNano() // 2020,8,5,13,14,15,999999999

// 获取当前年月日
carbon.Parse("2020-08-05 13:14:15.999999999").Date() // 2020,8,5
// 获取当前年月日毫秒
carbon.Parse("2020-08-05 13:14:15.999999999").DateMilli() // 2020,8,5,999
// 获取当前年月日微秒
carbon.Parse("2020-08-05 13:14:15.999999999").DateMicro() // 2020,8,5,999999
// 获取当前年月日纳秒
carbon.Parse("2020-08-05 13:14:15.999999999").DateNano() // 2020,8,5,999999999

// 获取当前时分秒
carbon.Parse("2020-08-05 13:14:15.999999999").Time() // 13,14,15
// 获取当前时分秒毫秒
carbon.Parse("2020-08-05 13:14:15.999999999").TimeMilli() // 13,14,15,999
// 获取当前时分秒微秒
carbon.Parse("2020-08-05 13:14:15.999999999").TimeMicro() // 13,14,15,999999
// 获取当前时分秒纳秒
carbon.Parse("2020-08-05 13:14:15.999999999").TimeNano() // 13,14,15,999999999

// 获取当前世纪
carbon.Parse("2020-08-05 13:14:15").Century() // 21
// 获取当前年代
carbon.Parse("2019-08-05 13:14:15").Decade() // 10
carbon.Parse("2021-08-05 13:14:15").Decade() // 20
// 获取当前年份
carbon.Parse("2020-08-05 13:14:15").Year() // 2020
// 获取当前季度
carbon.Parse("2020-08-05 13:14:15").Quarter() // 3
// 获取当前月份
carbon.Parse("2020-08-05 13:14:15").Month() // 8
// 获取当前周(从0开始)
carbon.Parse("2020-08-02 13:14:15").Week() // 0
carbon.Parse("2020-08-02").SetWeekStartsAt(carbon.Sunday).Week() // 0
carbon.Parse("2020-08-02").SetWeekStartsAt(carbon.Monday).Week() // 6
// 获取当前天数
carbon.Parse("2020-08-05 13:14:15").Day() // 5
// 获取当前小时
carbon.Parse("2020-08-05 13:14:15").Hour() // 13
// 获取当前分钟
carbon.Parse("2020-08-05 13:14:15").Minute() // 14
// 获取当前秒钟
carbon.Parse("2020-08-05 13:14:15").Second() // 15
// 获取当前毫秒
carbon.Parse("2020-08-05 13:14:15.999").Millisecond() // 999
// 获取当前微秒
carbon.Parse("2020-08-05 13:14:15.999").Microsecond() // 999000
// 获取当前纳秒
carbon.Parse("2020-08-05 13:14:15.999").Nanosecond() // 999000000

// 获取秒精度时间戳
carbon.Parse("2020-08-05 13:14:15").Timestamp() // 1596633255
// 获取毫秒精度时间戳
carbon.Parse("2020-08-05 13:14:15.999").TimestampMilli() // 1596633255999
// 获取微秒精度时间戳
carbon.Parse("2020-08-05 13:14:15.999999").TimestampMicro() // 1596633255999999
// 获取纳秒精度时间戳
carbon.Parse("2020-08-05 13:14:15.999999999").TimestampNano() // 1596633255999999999

// 获取时区位置
carbon.SetTimezone(carbon.PRC).Timezone() // PRC
carbon.SetTimezone(carbon.Tokyo).Timezone() // Asia/Tokyo

// 获取时区名称
carbon.SetTimezone(carbon.PRC).ZoneName() // CST
carbon.SetTimezone(carbon.Tokyo).ZoneName() // JST

// 获取时区偏移量，单位秒
carbon.SetTimezone(carbon.PRC).ZoneOffset() // 28800
carbon.SetTimezone(carbon.Tokyo).ZoneOffset() // 32400

// 获取当前区域
carbon.Now().Locale() // zh-CN
carbon.Now().SetLocale("en").Locale() // en

// 获取当前星座
carbon.Now().Constellation() // 狮子座
carbon.Now().SetLocale("en").Constellation() // Leo
carbon.Now().SetLocale("zh-CN").Constellation() // 狮子座

// 获取当前季节
carbon.Now().Season() // 夏季
carbon.Now().SetLocale("en").Season() // Summer
carbon.Now().SetLocale("zh-CN").Season() // 夏季

// 获取一周的开始日期
carbon.SetWeekStartsAt(carbon.Sunday).WeekStartsAt() // Sunday
carbon.SetWeekStartsAt(carbon.Monday).WeekStartsAt() // Monday

// 获取一周的结束日期
carbon.SetWeekStartsAt(carbon.Sunday).WeekEndsAt() // Saturday
carbon.SetWeekStartsAt(carbon.Monday).WeekEndsAt() // Sunday

// 获取当前布局模板
carbon.Parse("now").CurrentLayout() // "2006-01-02 15:04:05"
carbon.ParseByLayout("2020-08-05", DateLayout).CurrentLayout() // "2006-01-02"

// 获取年龄
carbon.Parse("2002-01-01 13:14:15").Age() // 17
carbon.Parse("2002-12-31 13:14:15").Age() // 18
```


---

## 时间设置

```go
// 设置时区
carbon.Parse("2020-08-05 13:14:15").SetTimezone(carbon.UTC).ToString() // 2020-08-05 13:14:15 +0000 UTC
carbon.Parse("2020-08-05 13:14:15").SetTimezone(carbon.PRC).ToString() // 2020-08-05 21:14:15 +0800 CST
carbon.Parse("2020-08-05 13:14:15").SetTimezone(carbon.Tokyo).ToString() // 2020-08-05 22:14:15 +0900 JST

// 设置位置
utc, _ := time.LoadLocation(carbon.UTC)
carbon.Parse("2020-08-05 13:14:15").SetLocation(utc).ToString() // 2020-08-05 13:14:15 +0000 UTC
prc, _ := time.LoadLocation(carbon.PRC)
carbon.Parse("2020-08-05 13:14:15").SetLocation(prc).ToString() // 2020-08-05 21:14:15 +0800 CST
tokyo, _ := time.LoadLocation(carbon.Tokyo)
carbon.Parse("2020-08-05 13:14:15").SetLocation(tokyo).ToString() // 2020-08-05 22:14:15 +0900 JST

// 设置区域
carbon.Parse("2020-07-05 13:14:15").SetLocale("en").DiffForHumans() // 1 month ago
carbon.Parse("2020-07-05 13:14:15").SetLocale("zh-CN").DiffForHumans() // 1 月前

// 设置年、月、日、时、分、秒
carbon.Parse("2020-01-01").SetDateTime(2019, 2, 2, 13, 14, 15).ToString() // 2019-02-02 13:14:15 +0000 UTC
carbon.Parse("2020-01-01").SetDateTime(2019, 2, 31, 13, 14, 15).ToString() // 2019-03-03 13:14:15 +0000 UTC
// 设置年、月、日、时、分、秒、毫秒
carbon.Parse("2020-01-01").SetDateTimeMilli(2019, 2, 2, 13, 14, 15, 999).ToString() // 2019-02-02 13:14:15.999 +0000 UTC
carbon.Parse("2020-01-01").SetDateTimeMilli(2019, 2, 31, 13, 14, 15, 999).ToString() // 2019-03-03 13:14:15.999 +0000 UTC
// 设置年、月、日、时、分、秒、微秒
carbon.Parse("2020-01-01").SetDateTimeMicro(2019, 2, 2, 13, 14, 15, 999999).ToString() // 2019-02-02 13:14:15.999999 +0000 UTC
carbon.Parse("2020-01-01").SetDateTimeMicro(2019, 2, 31, 13, 14, 15, 999999).ToString() // 2019-03-03 13:14:15.999999 +0000 UTC
// 设置年、月、日、时、分、秒、纳秒
carbon.Parse("2020-01-01").SetDateTimeNano(2019, 2, 2, 13, 14, 15, 999999999).ToString() // 2019-02-02 13:14:15.999999999 +0000 UTC
carbon.Parse("2020-01-01").SetDateTimeNano(2019, 2, 31, 13, 14, 15, 999999999).ToString() // 2019-03-03 13:14:15.999999999 +0000 UTC

// 设置年、月、日
carbon.Parse("2020-01-01").SetDate(2019, 2, 2).ToString() // 2019-02-02 00:00:00 +0000 UTC
carbon.Parse("2020-01-01").SetDate(2019, 2, 31).ToString() // 2019-03-03 00:00:00 +0000 UTC
// 设置年、月、日、毫秒
carbon.Parse("2020-01-01").SetDateMilli(2019, 2, 2, 999).ToString() // 2019-02-02 00:00:00.999 +0000 UTC
carbon.Parse("2020-01-01").SetDateMilli(2019, 2, 31, 999).ToString() // 2019-03-03 00:00:00.999 +0000 UTC
// 设置年、月、日、微秒
carbon.Parse("2020-01-01").SetDateMicro(2019, 2, 2, 999999).ToString() // 2019-02-02 00:00:00.999999 +0000 UTC
carbon.Parse("2020-01-01").SetDateMicro(2019, 2, 31, 999999).ToString() // 2019-03-03 00:00:00.999999 +0000 UTC
// 设置年、月、日、纳秒
carbon.Parse("2020-01-01").SetDateNano(2019, 2, 2, 999999999).ToString() // 2019-02-02 00:00:00.999999999 +0000 UTC
carbon.Parse("2020-01-01").SetDateNano(2019, 2, 31, 999999999).ToString() // 2019-03-03 00:00:00.999999999 +0000 UTC

// 设置时、分、秒
carbon.Parse("2020-01-01").SetTime(13, 14, 15).ToString() // 2020-01-01 13:14:15 +0000 UTC
carbon.Parse("2020-01-01").SetTime(13, 14, 90).ToString() // 2020-01-01 13:15:30 +0000 UTC
// 设置时、分、秒、毫秒
carbon.Parse("2020-01-01").SetTimeMilli(13, 14, 15, 999).ToString() // 2020-01-01 13:14:15.999 +0000 UTC
carbon.Parse("2020-01-01").SetTimeMilli(13, 14, 90, 999).ToString() // 2020-01-01 13:15:30.999 +0000 UTC
// 设置时、分、秒、微秒
carbon.Parse("2020-01-01").SetTimeMicro(13, 14, 15, 999999).ToString() // 2020-01-01 13:14:15.999999 +0000 UTC
carbon.Parse("2020-01-01").SetTimeMicro(13, 14, 90, 999999).ToString() // 2020-01-01 13:15:30.999999 +0000 UTC
// 设置时、分、秒、纳秒
carbon.Parse("2020-01-01").SetTimeNano(13, 14, 15, 999999999).ToString() // 2020-01-01 13:14:15.999999999 +0000 UTC
carbon.Parse("2020-01-01").SetTimeNano(13, 14, 90, 999999999).ToString() // 2020-01-01 13:15:30.999999999 +0000 UTC

// 设置年份
carbon.Parse("2020-02-29").SetYear(2021).ToDateString() // 2021-03-01
// 设置年份(月份不溢出)
carbon.Parse("2020-02-29").SetYearNoOverflow(2021).ToDateString() // 2021-02-28

// 设置月份
carbon.Parse("2020-01-31").SetMonth(2).ToDateString() // 2020-03-02
// 设置月份(月份不溢出)
carbon.Parse("2020-01-31").SetMonthNoOverflow(2).ToDateString() // 2020-02-29

// 设置一周的开始日期
carbon.Parse("2020-08-02").SetWeekStartsAt(carbon.Sunday).Week() // 0
carbon.Parse("2020-08-02").SetWeekStartsAt(carbon.Monday).Week() // 6

// 设置一周的周末日期
wd := []carbon.Weekday{
  carbon.Saturday, carbon.Sunday,
}
carbon.Parse("2025-04-11").SetWeekendDays(wd).IsWeekend() // false
carbon.Parse("2025-04-12").SetWeekendDays(wd).IsWeekend() // true
carbon.Parse("2025-04-13").SetWeekendDays(wd).IsWeekend() // true

// 设置日期
carbon.Parse("2019-08-05").SetDay(31).ToDateString() // 2020-08-31
carbon.Parse("2020-02-01").SetDay(31).ToDateString() // 2020-03-02

// 设置小时
carbon.Parse("2020-08-05 13:14:15").SetHour(10).ToDateTimeString() // 2020-08-05 10:14:15
carbon.Parse("2020-08-05 13:14:15").SetHour(24).ToDateTimeString() // 2020-08-06 00:14:15

// 设置分钟
carbon.Parse("2020-08-05 13:14:15").SetMinute(10).ToDateTimeString() // 2020-08-05 13:10:15
carbon.Parse("2020-08-05 13:14:15").SetMinute(60).ToDateTimeString() // 2020-08-05 14:00:15

// 设置秒
carbon.Parse("2020-08-05 13:14:15").SetSecond(10).ToDateTimeString() // 2020-08-05 13:14:10
carbon.Parse("2020-08-05 13:14:15").SetSecond(60).ToDateTimeString() // 2020-08-05 13:15:00

// 设置毫秒
carbon.Parse("2020-08-05 13:14:15").SetMillisecond(100).Millisecond() // 100
carbon.Parse("2020-08-05 13:14:15").SetMillisecond(999).Millisecond() // 999

// 设置微妙
carbon.Parse("2020-08-05 13:14:15").SetMicrosecond(100000).Microsecond() // 100000
carbon.Parse("2020-08-05 13:14:15").SetMicrosecond(999999).Microsecond() // 999999

// 设置纳秒
carbon.Parse("2020-08-05 13:14:15").SetNanosecond(100000000).Nanosecond() // 100000000
carbon.Parse("2020-08-05 13:14:15").SetNanosecond(999999999).Nanosecond() // 999999999
```


---

## 昨天、今天、明天
昨天是今天的前一天，明天是今天的后一天，等价于

```go
carbon.Yesterday() = carbon.Now().SubDay()
carbon.Tomorrow() = carbon.Now().AddDay()
```
假设当前时间为 `2020-08-05 13:14:15.999999999 +0000 UTC`

### 昨天
```go
// 昨天此刻
fmt.Printf("%s", carbon.Yesterday()) // 2020-08-04 13:14:15
carbon.Yesterday().String() // 2020-08-04 13:14:15
carbon.Yesterday().ToString() // 2020-08-04 13:14:15.999999999 +0000 UTC
carbon.Yesterday().ToDateTimeString() // 2020-08-04 13:14:15
// 昨天日期
carbon.Yesterday().ToDateString() // 2020-08-04
// 昨天时间
carbon.Yesterday().ToTimeString() // 13:14:15
// 指定时区的昨天此刻
carbon.Yesterday(carbon.NewYork).ToDateTimeString() // 2020-08-04 14:14:15
// 昨天秒精度时间戳
carbon.Yesterday().Timestamp() // 1596546855
// 昨天毫秒精度时间戳
carbon.Yesterday().TimestampMilli() // 1596546855999
// 昨天微秒精度时间戳
carbon.Yesterday().TimestampMicro() // 1596546855999999
// 昨天纳秒精度时间戳
carbon.Yesterday().TimestampNano() // 1596546855999999999
```

### 今天
```go
// 今天此刻
fmt.Printf("%s", carbon.Now()) // 2020-08-05 13:14:15
carbon.Now().String() // 2020-08-05 13:14:15
carbon.Now().ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC
carbon.Now().ToDateTimeString() // 2020-08-05 13:14:15
// 今天日期
carbon.Now().ToDateString() // 2020-08-05
// 今天时间
carbon.Now().ToTimeString() // 13:14:15
// 指定时区的今天此刻
carbon.Now(carbon.NewYork).ToDateTimeString() // 2020-08-05 14:14:15
// 今天秒精度时间戳
carbon.Now().Timestamp() // 1596633255
// 今天毫秒精度时间戳
carbon.Now().TimestampMilli() // 1596633255999
// 今天微秒精度时间戳
carbon.Now().TimestampMicro() // 1596633255999999
// 今天纳秒精度时间戳
carbon.Now().TimestampNano() // 1596633255999999999
```

### 明天
```go
// 明天此刻
fmt.Printf("%s", carbon.Tomorrow()) // 2020-08-06 13:14:15
carbon.Tomorrow().String() // 2020-08-06 13:14:15
carbon.Tomorrow().ToString() // 2020-08-06 13:14:15.999999999 +0000 UTC
carbon.Tomorrow().ToDateTimeString() // 2020-08-06 13:14:15
// 明天日期
carbon.Tomorrow().ToDateString() // 2020-08-06
// 明天时间
carbon.Tomorrow().ToTimeString() // 13:14:15
// 指定时区的明天此刻
carbon.Tomorrow(carbon.NewYork).ToDateTimeString() // 2020-08-06 14:14:15
// 明天秒精度时间戳
carbon.Tomorrow().Timestamp() // 1596719655
// 明天毫秒精度时间戳
carbon.Tomorrow().TimestampMilli() // 1596719655999
// 明天微秒精度时间戳
carbon.Tomorrow().TimestampMicro() // 1596719655999999
// 明天纳秒精度时间戳
carbon.Tomorrow().TimestampNano() // 1596719655999999999
```


---

## 时间旅行

### 世纪旅行
```go
// 三个世纪后
carbon.Parse("2020-02-29 13:14:15").AddCenturies(3).ToDateTimeString() // 2320-02-29 13:14:15
// 三个世纪后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddCenturiesNoOverflow(3).ToDateTimeString() // 2320-02-29 13:14:15
// 一个世纪后
carbon.Parse("2020-02-29 13:14:15").AddCentury().ToDateTimeString() // 2120-02-29 13:14:15
// 一个世纪后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddCenturyNoOverflow().ToDateTimeString() // 2120-02-29 13:14:15
// 三个世纪前
carbon.Parse("2020-02-29 13:14:15").SubCenturies(3).ToDateTimeString() // 1720-02-29 13:14:15
// 三个世纪前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubCenturiesNoOverflow(3).ToDateTimeString() // 1720-02-29 13:14:15
// 一个世纪前
carbon.Parse("2020-02-29 13:14:15").SubCentury().ToDateTimeString() // 1920-02-29 13:14:15
// 一世纪前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubCenturyNoOverflow().ToDateTimeString() // 1920-02-29 13:14:15
```

### 年代旅行
```go
// 三个年代后
carbon.Parse("2020-02-29 13:14:15").AddDecades(3).ToDateTimeString() // 2050-03-01 13:14:15
// 三个年代后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddDecadesNoOverflow(3).ToDateTimeString() // 2050-02-28 13:14:15
// 一个年代后
carbon.Parse("2020-02-29 13:14:15").AddDecade().ToDateTimeString() // 2030-03-01 13:14:15
// 一个年代后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddDecadeNoOverflow().ToDateTimeString() // 2030-02-28 13:14:15
// 三个年代前
carbon.Parse("2020-02-29 13:14:15").SubDecades(3).ToDateTimeString() // 1990-03-01 13:14:15
// 三个年代前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubDecadesNoOverflow(3).ToDateTimeString() // 1990-02-28 13:14:15
// 一个年代前
carbon.Parse("2020-02-29 13:14:15").SubDecade().ToDateTimeString() // 2010-03-01 13:14:15
// 一个年代前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubDecadeNoOverflow().ToDateTimeString() // 2010-02-28 13:14:15
```

### 年份旅行
```go
// 三年后
carbon.Parse("2020-02-29 13:14:15").AddYears(3).ToDateTimeString() // 2023-03-01 13:14:15
// 三年后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddYearsNoOverflow(3).ToDateTimeString() // 2023-02-28 13:14:15
// 一年后
carbon.Parse("2020-02-29 13:14:15").AddYear().ToDateTimeString() // 2021-03-01 13:14:15
// 一年后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddYearNoOverflow().ToDateTimeString() // 2021-02-28 13:14:15
// 三年前
carbon.Parse("2020-02-29 13:14:15").SubYears(3).ToDateTimeString() // 2017-03-01 13:14:15
// 三年前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubYearsNoOverflow(3).ToDateTimeString() // 2017-02-28 13:14:15
// 一年前
carbon.Parse("2020-02-29 13:14:15").SubYear().ToDateTimeString() // 2019-03-01 13:14:15
// 一年前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubYearNoOverflow().ToDateTimeString() // 2019-02-28 13:14:15
```

### 季度旅行
```go
// 三个季度后
carbon.Parse("2019-05-31 13:14:15").AddQuarters(3).ToDateTimeString() // 2020-03-02 13:14:15
// 三个季度后(月份不溢出)
carbon.Parse("2019-05-31 13:14:15").AddQuartersNoOverflow(3).ToDateTimeString() // 2020-02-29 13:14:15
// 一个季度后
carbon.Parse("2019-11-30 13:14:15").AddQuarter().ToDateTimeString() // 2020-03-01 13:14:15
// 一个季度后(月份不溢出)
carbon.Parse("2019-11-30 13:14:15").AddQuarterNoOverflow().ToDateTimeString() // 2020-02-29 13:14:15
// 三个季度前
carbon.Parse("2019-08-31 13:14:15").SubQuarters(3).ToDateTimeString() // 2019-03-03 13:14:15
// 三个季度前(月份不溢出)
carbon.Parse("2019-08-31 13:14:15").SubQuartersNoOverflow(3).ToDateTimeString() // 2019-02-28 13:14:15
// 一个季度前
carbon.Parse("2020-05-31 13:14:15").SubQuarter().ToDateTimeString() // 2020-03-02 13:14:15
// 一个季度前(月份不溢出)
carbon.Parse("2020-05-31 13:14:15").SubQuarterNoOverflow().ToDateTimeString() // 2020-02-29 13:14:15
```

### 月份旅行
```go
// 三个月后
carbon.Parse("2020-02-29 13:14:15").AddMonths(3).ToDateTimeString() // 2020-05-29 13:14:15
// 三个月后(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").AddMonthsNoOverflow(3).ToDateTimeString() // 2020-05-29 13:14:15
// 一个月后
carbon.Parse("2020-01-31 13:14:15").AddMonth().ToDateTimeString() // 2020-03-02 13:14:15
// 一个月后(月份不溢出)
carbon.Parse("2020-01-31 13:14:15").AddMonthNoOverflow().ToDateTimeString() // 2020-02-29 13:14:15
// 三个月前
carbon.Parse("2020-02-29 13:14:15").SubMonths(3).ToDateTimeString() // 2019-11-29 13:14:15
// 三个月前(月份不溢出)
carbon.Parse("2020-02-29 13:14:15").SubMonthsNoOverflow(3).ToDateTimeString() // 2019-11-29 13:14:15
// 一个月前
carbon.Parse("2020-03-31 13:14:15").SubMonth().ToDateTimeString() // 2020-03-02 13:14:15
// 一个月前(月份不溢出)
carbon.Parse("2020-03-31 13:14:15").SubMonthNoOverflow().ToDateTimeString() // 2020-02-29 13:14:15
```

### 周旅行
```go
// 三周后
carbon.Parse("2020-02-29 13:14:15").AddWeeks(3).ToDateTimeString() // 2020-03-21 13:14:15
// 一周后
carbon.Parse("2020-02-29 13:14:15").AddWeek().ToDateTimeString() // 2020-03-07 13:14:15
// 三周前
carbon.Parse("2020-02-29 13:14:15").SubWeeks(3).ToDateTimeString() // 2020-02-08 13:14:15
// 一周前
carbon.Parse("2020-02-29 13:14:15").SubWeek().ToDateTimeString() // 2020-02-22 13:14:15
```

### 天旅行
```go
// 三天后
carbon.Parse("2020-08-05 13:14:15").AddDays(3).ToDateTimeString() // 2020-08-08 13:14:15
// 一天后
carbon.Parse("2020-08-05 13:14:15").AddDay().ToDateTimeString() // 2020-08-05 13:14:15
// 三天前
carbon.Parse("2020-08-05 13:14:15").SubDays(3).ToDateTimeString() // 2020-08-02 13:14:15
// 一天前
carbon.Parse("2020-08-05 13:14:15").SubDay().ToDateTimeString() // 2020-08-04 13:14:15
```

### 小时旅行
```go
// 三小时后
carbon.Parse("2020-08-05 13:14:15").AddHours(3).ToDateTimeString() // 2020-08-05 16:14:15
// 二小时半后
carbon.Parse("2020-08-05 13:14:15").AddDuration("2.5h").ToDateTimeString() // 2020-08-05 15:44:15
carbon.Parse("2020-08-05 13:14:15").AddDuration("2h30m").ToDateTimeString() // 2020-08-05 15:44:15
// 一小时后
carbon.Parse("2020-08-05 13:14:15").AddHour().ToDateTimeString() // 2020-08-05 14:14:15
// 三小时前
carbon.Parse("2020-08-05 13:14:15").SubHours(3).ToDateTimeString() // 2020-08-05 10:14:15
// 二小时半前
carbon.Parse("2020-08-05 13:14:15").SubDuration("2.5h").ToDateTimeString() // 2020-08-05 10:44:15
carbon.Parse("2020-08-05 13:14:15").SubDuration("2h30m").ToDateTimeString() // 2020-08-05 10:44:15
// 一小时前
carbon.Parse("2020-08-05 13:14:15").SubHour().ToDateTimeString() // 2020-08-05 12:14:15
```

### 分钟旅行
```go
// 三分钟后
carbon.Parse("2020-08-05 13:14:15").AddMinutes(3).ToDateTimeString() // 2020-08-05 13:17:15
// 二分钟半后
carbon.Parse("2020-08-05 13:14:15").AddDuration("2.5m").ToDateTimeString() // 2020-08-05 13:16:45
carbon.Parse("2020-08-05 13:14:15").AddDuration("2m30s").ToDateTimeString() // 2020-08-05 13:16:45
// 一分钟后
carbon.Parse("2020-08-05 13:14:15").AddMinute().ToDateTimeString() // 2020-08-05 13:15:15
// 三分钟前
carbon.Parse("2020-08-05 13:14:15").SubMinutes(3).ToDateTimeString() // 2020-08-05 13:11:15
// 二分钟半前
carbon.Parse("2020-08-05 13:14:15").SubDuration("2.5m").ToDateTimeString() // 2020-08-05 13:11:45
carbon.Parse("2020-08-05 13:14:15").SubDuration("2m30s").ToDateTimeString() // 2020-08-05 13:11:45
// 一分钟前
carbon.Parse("2020-08-05 13:14:15").SubMinute().ToDateTimeString() // 2020-08-05 13:13:15
```

### 秒钟旅行
```go
// 三秒钟后
carbon.Parse("2020-08-05 13:14:15").AddSeconds(3).ToDateTimeString() // 2020-08-05 13:14:18
// 二秒钟半后
carbon.Parse("2020-08-05 13:14:15").AddDuration("2.5s").ToDateTimeString() // 2020-08-05 13:14:17
// 一秒钟后
carbon.Parse("2020-08-05 13:14:15").AddSecond().ToDateTimeString() // 2020-08-05 13:14:16
// 三秒钟前
carbon.Parse("2020-08-05 13:14:15").SubSeconds(3).ToDateTimeString() // 2020-08-05 13:14:12
// 二秒钟半前
carbon.Parse("2020-08-05 13:14:15").SubDuration("2.5s").ToDateTimeString() // 2020-08-05 13:14:12
// 一秒钟前
carbon.Parse("2020-08-05 13:14:15").SubSecond().ToDateTimeString() // 2020-08-05 13:14:14
```

### 毫秒旅行
```go
// 三毫秒后
carbon.Parse("2020-08-05 13:14:15.222222222").AddMilliseconds(3).ToString() // 2020-08-05 13:14:15.225222222 +0000 UTC
// 一毫秒后
carbon.Parse("2020-08-05 13:14:15.222222222").AddMillisecond().ToString() // 2020-08-05 13:14:15.223222222 +0000 UTC
// 三毫秒前
carbon.Parse("2020-08-05 13:14:15.222222222").SubMilliseconds(3).ToString() // 2020-08-05 13:14:15.219222222 +0000 UTC
// 一毫秒前
carbon.Parse("2020-08-05 13:14:15.222222222").SubMillisecond().ToString() // 2020-08-05 13:14:15.221222222 +0000 UTC
```

### 微秒旅行
```go
// 三微秒后
carbon.Parse("2020-08-05 13:14:15.222222222").AddMicroseconds(3).ToString() // 2020-08-05 13:14:15.222225222 +0000 UTC
// 一微秒后
carbon.Parse("2020-08-05 13:14:15.222222222").AddMicrosecond().ToString() // 2020-08-05 13:14:15.222223222 +0000 UTC
// 三微秒前
carbon.Parse("2020-08-05 13:14:15.222222222").SubMicroseconds(3).ToString() // 2020-08-05 13:14:15.222219222 +0000 UTC
// 一微秒前
carbon.Parse("2020-08-05 13:14:15.222222222").SubMicrosecond().ToString() // 2020-08-05 13:14:15.222221222 +0000 UTC
```

### 纳秒旅行
```go
// 三纳秒后
carbon.Parse("2020-08-05 13:14:15.222222222").AddNanoseconds(3).ToString() // 2020-08-05 13:14:15.222222225 +0000 UTC
// 一纳秒后
carbon.Parse("2020-08-05 13:14:15.222222222").AddNanosecond().ToString() // 2020-08-05 13:14:15.222222223 +0000 UTC
// 三纳秒前
carbon.Parse("2020-08-05 13:14:15.222222222").SubNanoseconds(3).ToString() // 2020-08-05 13:14:15.222222219 +0000 UTC
// 一纳秒前
carbon.Parse("2020-08-05 13:14:15.222222222").SubNanosecond().ToString() // 2020-08-05 13:14:15.222222221 +0000 UTC
```


---

## 时间边界

### 世纪边界
```go
// 本世纪开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfCentury().ToDateTimeString() // 2000-01-01 00:00:00
// 本世纪结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfCentury().ToDateTimeString() // 2999-12-31 23:59:59
```

### 年代边界
```go
// 本年代开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfDecade().ToDateTimeString() // 2020-01-01 00:00:00
carbon.Parse("2021-08-05 13:14:15").StartOfDecade().ToDateTimeString() // 2020-01-01 00:00:00
carbon.Parse("2029-08-05 13:14:15").StartOfDecade().ToDateTimeString() // 2020-01-01 00:00:00
// 本年代结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfDecade().ToDateTimeString() // 2029-12-31 23:59:59
carbon.Parse("2021-08-05 13:14:15").EndOfDecade().ToDateTimeString() // 2029-12-31 23:59:59
carbon.Parse("2029-08-05 13:14:15").EndOfDecade().ToDateTimeString() // 2029-12-31 23:59:59
```

### 年份边界
```go
// 本年开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfYear().ToDateTimeString() // 2020-01-01 00:00:00
// 本年结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfYear().ToDateTimeString() // 2020-12-31 23:59:59
```

### 季度边界
```go
// 本季度开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfQuarter().ToDateTimeString() // 2020-07-01 00:00:00
// 本季度结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfQuarter().ToDateTimeString() // 2020-09-30 23:59:59
```

### 月份边界
```go
// 本月开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfMonth().ToDateTimeString() // 2020-08-01 00:00:00
// 本月结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfMonth().ToDateTimeString() // 2020-08-31 23:59:59
```

### 周边界
```go
// 本周开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfWeek().ToDateTimeString() // 2020-08-02 00:00:00
carbon.Parse("2020-08-05 13:14:15").SetWeekStartsAt(carbon.Sunday).StartOfWeek().ToDateTimeString() // 2020-08-02 00:00:00
carbon.Parse("2020-08-05 13:14:15").SetWeekStartsAt(carbon.Monday).StartOfWeek().ToDateTimeString() // 2020-08-03 00:00:00
// 本周结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfWeek().ToDateTimeString() // 2020-08-08 23:59:59
carbon.Parse("2020-08-05 13:14:15").SetWeekStartsAt(carbon.Sunday).EndOfWeek().ToDateTimeString() // 2020-08-08 23:59:59
carbon.Parse("2020-08-05 13:14:15").SetWeekStartsAt(carbon.Monday).EndOfWeek().ToDateTimeString() // 2020-08-09 23:59:59
```

### 日边界
```go
// 本日开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfDay().ToDateTimeString() // 2020-08-05 00:00:00
// 本日结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfDay().ToDateTimeString() // 2020-08-05 23:59:59
```

### 小时边界
```go
// 本小时开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfHour().ToDateTimeString() // 2020-08-05 13:00:00
// 本小时结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfHour().ToDateTimeString() // 2020-08-05 13:59:59
```

### 分钟边界
```go
// 本分钟开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfMinute().ToDateTimeString() // 2020-08-05 13:14:00
// 本分钟结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfMinute().ToDateTimeString() // 2020-08-05 13:14:59
```

### 秒钟边界
```go
// 本秒开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfSecond().ToString() // 2020-08-05 13:14:15 +0000 UTC
// 本秒结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfSecond().ToString() // 2020-08-05 13:14:15.999999999 +0000 UTC
```


---

## 时间差值

假设当前时间为 `2020-08-05 13:14:15.999999999 +0000 UTC`
```go
carbon.SetTestNow(carbon.Parse("2020-08-05 13:14:15.999999999 +0000 UTC"))
```

### 相对差值
```go
carbon.Parse("2021-08-05 13:14:15").DiffInYears(carbon.Parse("2020-08-05 13:14:15")) // -1
carbon.Parse("2020-08-05 13:14:15").DiffInMonths(carbon.Parse("2020-07-05 13:14:15")) // -1
carbon.Parse("2020-08-05 13:14:15").DiffInWeeks(carbon.Parse("2020-07-28 13:14:15")) // -1
carbon.Parse("2020-08-05 13:14:15").DiffInDays(carbon.Parse("2020-08-04 13:14:15")) // -1
carbon.Parse("2020-08-05 13:14:15").DiffInHours(carbon.Parse("2020-08-05 12:14:15")) // -1
carbon.Parse("2020-08-05 13:14:15").DiffInMinutes(carbon.Parse("2020-08-05 13:13:15")) // -1
carbon.Parse("2020-08-05 13:14:15").DiffInSeconds(carbon.Parse("2020-08-05 13:14:14")) // -1

carbon.Now().DiffInString() // just now
carbon.Now().AddYearsNoOverflow(1).DiffInString() // -1 year
carbon.Now().SubYearsNoOverflow(1).DiffInString() // 1 year

now := carbon.Now()
now.DiffInDuration(now).String() // 0s
now.AddHour().DiffInDuration(now).String() // 1h0m0s
now.SubHour().DiffInDuration(now).String() // -1h0m0s
```

### 绝对差值
```go
carbon.Parse("2021-08-05 13:14:15").DiffAbsInYears(carbon.Parse("2020-08-05 13:14:15")) // 1
carbon.Parse("2020-08-05 13:14:15").DiffAbsInMonths(carbon.Parse("2020-07-05 13:14:15")) // 1
carbon.Parse("2020-08-05 13:14:15").DiffAbsInWeeks(carbon.Parse("2020-07-28 13:14:15")) // 1
carbon.Parse("2020-08-05 13:14:15").DiffAbsInDays(carbon.Parse("2020-08-04 13:14:15")) // 1
carbon.Parse("2020-08-05 13:14:15").DiffAbsInHours(carbon.Parse("2020-08-05 12:14:15")) // 1
carbon.Parse("2020-08-05 13:14:15").DiffAbsInMinutes(carbon.Parse("2020-08-05 13:13:15")) // 1
carbon.Parse("2020-08-05 13:14:15").DiffAbsInSeconds(carbon.Parse("2020-08-05 13:14:14")) // 1

carbon.Now().DiffAbsInString(carbon.Now()) // just now
carbon.Now().AddYearsNoOverflow(1).DiffAbsInString(carbon.Now()) // 1 year
carbon.Now().SubYearsNoOverflow(1).DiffAbsInString(carbon.Now()) // 1 year

now.DiffAbsInDuration(now).String() // 0s
now.AddHour().DiffAbsInDuration(now).String() // 1h0m0s
now.SubHour().DiffAbsInDuration(now).String() // 1h0m0s
```

### 人性化差值
```go

carbon.Parse("2020-08-05 13:14:15").DiffForHumans() // just now
carbon.Parse("2019-08-05 13:14:15").DiffForHumans() // 1 year ago
carbon.Parse("2018-08-05 13:14:15").DiffForHumans() // 2 years ago
carbon.Parse("2021-08-05 13:14:15").DiffForHumans() // 1 year from now
carbon.Parse("2022-08-05 13:14:15").DiffForHumans() // 2 years from now

carbon.SetLocale("zh-CN")
carbon.Parse("2020-08-05 13:14:15").DiffForHumans() // 刚刚
carbon.Parse("2019-08-05 13:14:15").DiffForHumans() // 1 年前
carbon.Parse("2018-08-05 13:14:15").DiffForHumans() // 2 年前
carbon.Parse("2021-08-05 13:14:15").DiffForHumans() // 1 年后
carbon.Parse("2022-08-05 13:14:15").DiffForHumans() // 2 年后
```


---

## 时间判断

### 含有判断
```go
// 是否有错误
carbon.NewCarbon().HasError() // false
carbon.ZeroValue().HasError() // false
carbon.EpochValue().HasError() // false
carbon.CreateFromTimestamp(0).HasError() // false
carbon.Parse("").HasError() // false
carbon.Parse("0").HasError() // true
carbon.Parse("xxx").HasError() // true
carbon.Parse("2020-08-05").HasError() // false
```

### 是否判断
```go
// 是否是零值时间(0001-01-01 00:00:00 +0000 UTC)
carbon.NewCarbon().IsZero() // true
carbon.ZeroValue().IsZero() // true
carbon.EpochValue().IsZero() // false
carbon.CreateFromTimestamp(0).IsZero() // false
carbon.Parse("").IsZero() // false
carbon.Parse("0").IsZero() // false
carbon.Parse("xxx").IsZero() // false
carbon.Parse("2020-08-05").IsZero() // false

// 是否是空值
carbon.NewCarbon().IsEmpty() // false
carbon.ZeroValue().IsEmpty() // false
carbon.EpochValue().IsEmpty() // false
carbon.CreateFromTimestamp(0).IsEmpty() // false
carbon.Parse("").IsEmpty() // true
carbon.Parse("0").IsEmpty() // false
carbon.Parse("xxx").IsEmpty() // false
carbon.Parse("2020-08-05").IsEmpty() // false

// 是否是 UNIX 纪元时间(1970-01-01 00:00:00 +0000 UTC)
carbon.NewCarbon().IsEpoch() // false
carbon.ZeroValue().IsEpoch() // false
carbon.EpochValue().IsEpoch() // true
carbon.CreateFromTimestamp(0).IsEpoch() // true
carbon.Parse("").IsEpoch() // false
carbon.Parse("0").IsEpoch() // false
carbon.Parse("xxx").IsEpoch() // false
carbon.Parse("2020-08-05").IsEpoch() // false

// 是否是有效时间
carbon.NewCarbon().IsValid() // true
carbon.ZeroValue().IsValid() // true
carbon.EpochValue().IsEpoch() // true
carbon.CreateFromTimestamp(0).IsValid() // true
carbon.Parse("").IsValid() // false
carbon.Parse("0").IsValid() // false
carbon.Parse("xxx").IsValid() // false
carbon.Parse("0000-00-00 00:00:00").IsValid() // false
carbon.Parse("0000-00-00").IsValid() // false
carbon.Parse("00:00:00").IsValid() // false
carbon.Parse("2020-08-05 00:00:00").IsValid() // true
carbon.Parse("2020-08-05").IsValid() // true

// 是否是无效时间
carbon.NewCarbon().IsInvalid() // false
carbon.ZeroValue().IsInvalid() // false
carbon.EpochValue().IsInvalid() // false
carbon.CreateFromTimestamp(0).IsInvalid() // false
carbon.Parse("").IsInvalid() // false
carbon.Parse("0").IsInvalid() // true
carbon.Parse("xxx").IsInvalid() // true
carbon.Parse("0000-00-00 00:00:00").IsInvalid() // true
carbon.Parse("0000-00-00").IsInvalid() // true
carbon.Parse("00:00:00").IsInvalid() // true
carbon.Parse("2020-08-05 00:00:00").IsInvalid() // false
carbon.Parse("2020-08-05").IsInvalid() // false

// 是否是夏令时
carbon.Parse("").IsDST() // false
carbon.Parse("0").IsDST() // false
carbon.Parse("xxx").IsDST() // false
carbon.Parse("0000-00-00 00:00:00").IsDST() // false
carbon.Parse("0000-00-00").IsDST() // false
carbon.Parse("00:00:00").IsDST() // false
carbon.Parse("2023-01-01", "Australia/Brisbane").IsDST() // false
carbon.Parse("2023-01-01", "Australia/Sydney").IsDST() // true

// 是否是上午
carbon.Parse("2020-08-05 00:00:00").IsAM() // true
carbon.Parse("2020-08-05 08:00:00").IsAM() // true
carbon.Parse("2020-08-05 12:00:00").IsAM() // false
carbon.Parse("2020-08-05 13:00:00").IsAM() // false
// 是否是下午
carbon.Parse("2020-08-05 00:00:00").IsPM() // false
carbon.Parse("2020-08-05 08:00:00").IsPM() // false
carbon.Parse("2020-08-05 12:00:00").IsPM() // true
carbon.Parse("2020-08-05 13:00:00").IsPM() // true

// 是否是当前时间
carbon.Now().IsNow() // true
// 是否是未来时间
carbon.Tomorrow().IsFuture() // true
// 是否是过去时间
carbon.Yesterday().IsPast() // true

// 是否是闰年
carbon.Parse("2020-08-05 13:14:15").IsLeapYear() // true
// 是否是长年
carbon.Parse("2020-08-05 13:14:15").IsLongYear() // true

// 是否是一月
carbon.Parse("2020-08-05 13:14:15").IsJanuary() // false
// 是否是二月
carbon.Parse("2020-08-05 13:14:15").IsFebruary() // false
// 是否是三月
carbon.Parse("2020-08-05 13:14:15").IsMarch() // false
// 是否是四月
carbon.Parse("2020-08-05 13:14:15").IsApril() // false
// 是否是五月
carbon.Parse("2020-08-05 13:14:15").IsMay() // false
// 是否是六月
carbon.Parse("2020-08-05 13:14:15").IsJune() // false
// 是否是七月
carbon.Parse("2020-08-05 13:14:15").IsJuly() // false
// 是否是八月
carbon.Parse("2020-08-05 13:14:15").IsAugust() // false
// 是否是九月
carbon.Parse("2020-08-05 13:14:15").IsSeptember() // true
// 是否是十月
carbon.Parse("2020-08-05 13:14:15").IsOctober() // false
// 是否是十一月
carbon.Parse("2020-08-05 13:14:15").IsNovember() // false
// 是否是十二月
carbon.Parse("2020-08-05 13:14:15").IsDecember() // false

// 是否是周一
carbon.Parse("2020-08-05 13:14:15").IsMonday() // false
// 是否是周二
carbon.Parse("2020-08-05 13:14:15").IsTuesday() // true
// 是否是周三
carbon.Parse("2020-08-05 13:14:15").IsWednesday() // false
// 是否是周四
carbon.Parse("2020-08-05 13:14:15").IsThursday() // false
// 是否是周五
carbon.Parse("2020-08-05 13:14:15").IsFriday() // false
// 是否是周六
carbon.Parse("2020-08-05 13:14:15").IsSaturday() // false
// 是否是周日
carbon.Parse("2020-08-05 13:14:15").IsSunday() // false

// 是否是工作日
carbon.Parse("2020-08-05 13:14:15").IsWeekday() // false
// 是否是周末
carbon.Parse("2020-08-05 13:14:15").IsWeekend() // true

// 是否是昨天
carbon.Parse("2020-08-04 13:14:15").IsYesterday() // true
carbon.Parse("2020-08-04 00:00:00").IsYesterday() // true
carbon.Parse("2020-08-04").IsYesterday() // true
// 是否是今天
carbon.Parse("2020-08-05 13:14:15").IsToday() // true
carbon.Parse("2020-08-05 00:00:00").IsToday() // true
carbon.Parse("2020-08-05").IsToday() // true
// 是否是明天
carbon.Parse("2020-08-06 13:14:15").IsTomorrow() // true
carbon.Parse("2020-08-06 00:00:00").IsTomorrow() // true
carbon.Parse("2020-08-06").IsTomorrow() // true

// 是否是同一世纪
carbon.Parse("2020-08-05 13:14:15").IsSameCentury(carbon.Parse("3020-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").IsSameCentury(carbon.Parse("2099-08-05 13:14:15")) // true
// 是否是同一年代
carbon.Parse("2020-08-05 13:14:15").IsSameDecade(carbon.Parse("2030-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").IsSameDecade(carbon.Parse("2120-08-05 13:14:15")) // true
// 是否是同一年
carbon.Parse("2020-08-05 00:00:00").IsSameYear(carbon.Parse("2021-08-05 13:14:15")) // false
carbon.Parse("2020-01-01 00:00:00").IsSameYear(carbon.Parse("2020-12-31 13:14:15")) // true
// 是否是同一季节
carbon.Parse("2020-08-05 00:00:00").IsSameQuarter(carbon.Parse("2020-09-05 13:14:15")) // false
carbon.Parse("2020-01-01 00:00:00").IsSameQuarter(carbon.Parse("2021-01-31 13:14:15")) // true
// 是否是同一月
carbon.Parse("2020-01-01 00:00:00").IsSameMonth(carbon.Parse("2021-01-31 13:14:15")) // false
carbon.Parse("2020-01-01 00:00:00").IsSameMonth(carbon.Parse("2020-01-31 13:14:15")) // true
// 是否是同一天
carbon.Parse("2020-08-05 13:14:15").IsSameDay(carbon.Parse("2021-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 00:00:00").IsSameDay(carbon.Parse("2020-08-05 13:14:15")) // true
// 是否是同一小时
carbon.Parse("2020-08-05 13:14:15").IsSameHour(carbon.Parse("2021-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:00:00").IsSameHour(carbon.Parse("2020-08-05 13:14:15")) // true
// 是否是同一分钟
carbon.Parse("2020-08-05 13:14:15").IsSameMinute(carbon.Parse("2021-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:00").IsSameMinute(carbon.Parse("2020-08-05 13:14:15")) // true
// 是否是同一秒
carbon.Parse("2020-08-05 13:14:15").IsSameSecond(carbon.Parse("2021-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").IsSameSecond(carbon.Parse("2020-08-05 13:14:15")) // true
```

### 数学判断
```go
// 是否大于
carbon.Parse("2020-08-05 13:14:15").Gt(carbon.Parse("2020-08-04 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Gt(carbon.Parse("2020-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").Compare(">", carbon.Parse("2020-08-04 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare(">", carbon.Parse("2020-08-05 13:14:15")) // false

// 是否小于
carbon.Parse("2020-08-05 13:14:15").Lt(carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Lt(carbon.Parse("2020-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").Compare("<", carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare("<", carbon.Parse("2020-08-05 13:14:15")) // false

// 是否等于
carbon.Parse("2020-08-05 13:14:15").Eq(carbon.Parse("2020-08-05 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Eq(carbon.Parse("2020-08-05 13:14:00")) // false
carbon.Parse("2020-08-05 13:14:15").Compare("=", carbon.Parse("2020-08-05 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare("=", carbon.Parse("2020-08-05 13:14:00")) // false

// 是否不等于
carbon.Parse("2020-08-05 13:14:15").Ne(carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Ne(carbon.Parse("2020-08-05 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").Compare("!=", carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare("<>", carbon.Parse("2020-08-05 13:14:15")) // false

// 是否大于等于
carbon.Parse("2020-08-05 13:14:15").Gte(carbon.Parse("2020-08-04 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Gte(carbon.Parse("2020-08-05 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare(">=", carbon.Parse("2020-08-04 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare(">=", carbon.Parse("2020-08-05 13:14:15")) // true

// 是否小于等于
carbon.Parse("2020-08-05 13:14:15").Lte(carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Lte(carbon.Parse("2020-08-05 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare("<=", carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").Compare("<=", carbon.Parse("2020-08-05 13:14:15")) // true
```

### 距离判断
```go
// 是否在两个时间之间(不包括这两个时间)
carbon.Parse("2020-08-05 13:14:15").Between(carbon.Parse("2020-08-05 13:14:15"), carbon.Parse("2020-08-06 13:14:15")) // false
carbon.Parse("2020-08-05 13:14:15").Between(carbon.Parse("2020-08-04 13:14:15"), carbon.Parse("2020-08-06 13:14:15")) // true

// 是否在两个时间之间(包括开始时间)
carbon.Parse("2020-08-05 13:14:15").BetweenIncludedStart(carbon.Parse("2020-08-05 13:14:15"), carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").BetweenIncludedStart(carbon.Parse("2020-08-04 13:14:15"), carbon.Parse("2020-08-06 13:14:15")) // true

// 是否在两个时间之间(包括结束时间)
carbon.Parse("2020-08-05 13:14:15").BetweenIncludedEnd(carbon.Parse("2020-08-04 13:14:15"), carbon.Parse("2020-08-05 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").BetweenIncludedEnd(carbon.Parse("2020-08-04 13:14:15"), carbon.Parse("2020-08-06 13:14:15")) // true

// 是否在两个时间之间(包括这两个时间)
carbon.Parse("2020-08-05 13:14:15").BetweenIncludedBoth(carbon.Parse("2020-08-05 13:14:15"), carbon.Parse("2020-08-06 13:14:15")) // true
carbon.Parse("2020-08-05 13:14:15").BetweenIncludedBoth(carbon.Parse("2020-08-04 13:14:15"), carbon.Parse("2020-08-05 13:14:15")) // true
```


---

## 时间极值

### 极值判断
```go
c1 := carbon.Parse("2020-08-01")
c2 := carbon.Parse("2020-08-05")
c3 := carbon.Parse("2020-08-06")

// 返回最大的 Carbon 实例
carbon.Max(c1, c2, c3) // c3
// 返回最小的 Carbon 实例
carbon.Min(c1, c2, c3) // c1

c := carbon.Parse("2020-07-01")
// 返回最近的 Carbon 实例
c.Closest(c1, c2, c3) // c1
// 返回最远的 Carbon 实例
c.Farthest(c1, c2, c3) // c3
```

### 极值边界
```go
// 返回零值 Carbon
carbon.ZeroValue().ToString() // 0001-01-01 00:00:00 +0000 UTC
// 返回 linux 纪元值 Carbon
carbon.EpochValue().ToString() // 1970-01-01 00:00:00 +0000 UTC

// 返回 Carbon 的最大值
carbon.MaxValue().ToString() // 9999-12-31 23:59:59.999999999 +0000 UTC
// 返回 Carbon 的最小值
carbon.MinValue().ToString() // 0001-01-01 00:00:00 +0000 UTC

// 返回 Duration 的最大值
carbon.MaxDuration().Seconds() // 9.223372036854776e+09
// 返回 Duration 的最小值
carbon.MinDuration().Seconds() // -9.223372036854776e+09
```


---

## 星座

### 星座名称
```go
carbon.Parse("2020-08-05 13:14:15").Constellation() // Leo
carbon.Parse("2020-08-05 13:14:15").SetLocale("zh-CN").Constellation() // 狮子座
```

### 星座判断
```go
// 是否是白羊座
carbon.Parse("2020-08-05 13:14:15").IsAries() // false
// 是否是金牛座
carbon.Parse("2020-08-05 13:14:15").IsTaurus() // false
// 是否是双子座
carbon.Parse("2020-08-05 13:14:15").IsGemini() // false
// 是否是巨蟹座
carbon.Parse("2020-08-05 13:14:15").IsCancer() // false
// 是否是狮子座
carbon.Parse("2020-08-05 13:14:15").IsLeo() // true
// 是否是处女座
carbon.Parse("2020-08-05 13:14:15").IsVirgo() // false
// 是否是天秤座
carbon.Parse("2020-08-05 13:14:15").IsLibra() // false
// 是否是天蝎座
carbon.Parse("2020-08-05 13:14:15").IsScorpio() // false
// 是否是射手座
carbon.Parse("2020-08-05 13:14:15").IsSagittarius() // false
// 是否是摩羯座
carbon.Parse("2020-08-05 13:14:15").IsCapricorn() // false
// 是否是水瓶座
carbon.Parse("2020-08-05 13:14:15").IsAquarius() // false
// 是否是双鱼座
carbon.Parse("2020-08-05 13:14:15").IsPisces() // false
```


---

## 季节
按照气象划分，即 `3-5`月为 `春季`，`6-8`月为 `夏季`，`9-11`月为 `秋季`，12-2月为 `冬季`

### 季节名称
```go
carbon.Parse("2020-08-05 13:14:15").Season() // Summer
carbon.Parse("2020-08-05 13:14:15").SetLocale("zh-CN").Season() // 夏季
```

### 季节边界
```go
// 本季节开始时间
carbon.Parse("2020-08-05 13:14:15").StartOfSeason().ToDateTimeString() // 2020-06-01 00:00:00
// 本季节结束时间
carbon.Parse("2020-08-05 13:14:15").EndOfSeason().ToDateTimeString() // 2020-08-31 23:59:59
```

### 季节判断
```go
// 是否是春季
carbon.Parse("2020-08-05 13:14:15").IsSpring() // false
// 是否是夏季
carbon.Parse("2020-08-05 13:14:15").IsSummer() // true
// 是否是秋季
carbon.Parse("2020-08-05 13:14:15").IsAutumn() // false
// 是否是冬季
carbon.Parse("2020-08-05 13:14:15").IsWinter() // false
```


---

## 日历

目前支持的日历有
- [儒略日/简化儒略日](#儒略日-简化儒略日)
- [中国农历](#中国农历)
- [波斯历(伊朗历)](#波斯历-伊朗历)
- [希伯来历(犹太历)](#希伯来历-犹太历)

### 儒略日/简化儒略日

#### 将 `公历` 转换成 `儒略日`
```go
// 默认保留 6 位小数精度
carbon.Parse("2024-01-24 12:00:00").Julian().JD() // 2460334
carbon.Parse("2024-01-24 13:14:15").Julian().JD() // 2460334.051563

// 保留 4 位小数精度
carbon.Parse("2024-01-24 12:00:00").Julian().JD(4) // 2460334
carbon.Parse("2024-01-24 13:14:15").Julian().JD(4) // 2460334.0516
```

#### 将 `公历` 转换成 `简化儒略日`
```go
// 默认保留 6 位小数精度
carbon.Parse("2024-01-24 12:00:00").Julian().MJD() // 60333.5
carbon.Parse("2024-01-24 13:14:15").Julian().MJD() // 60333.551563

// 保留 4 位小数精度
carbon.Parse("2024-01-24 12:00:00").Julian().MJD(4) // 60333.5
carbon.Parse("2024-01-24 13:14:15").Julian().MJD(4) // 60333.5516
```

#### 将 `儒略日` 转换成 `简化儒略日`
```go
// 默认保留 6 位小数精度
carbon.CreateFromJulian(2460334).Julian().MJD() // 60333.5
carbon.CreateFromJulian(2460334.051563).Julian().MJD() // 60332.551563

// 保留 4 位小数精度
carbon.CreateFromJulian(2460334).Julian().MJD(4) // 60333.5
carbon.CreateFromJulian(2460334.051563).Julian().MJD(4) // 60332.5516
```

#### 将 `简化儒略日` 转换成 `儒略日`
```go
// 默认保留 6 位小数精度
carbon.CreateFromJulian(60333.5).Julian().JD()() // 2460334
carbon.CreateFromJulian(60333.551563).Julian().JD()() // 2460333.051563

// 保留 4 位小数精度
carbon.CreateFromJulian(60333.5).Julian().JD(4) // 2460334
carbon.CreateFromJulian(60333.551563).Julian().JD(4) // 2460333.0516
```

#### 将 `儒略日`/`简化儒略日` 转换成 `公历`
```go
// 将 儒略日 转换成 公历
carbon.CreateFromJulian(2460334).ToDateTimeString() // 2024-01-24 12:00:00
carbon.CreateFromJulian(2460334.051563).ToDateTimeString() // 2024-01-24 13:14:15

// 将 简化儒略日 转换成 公历
carbon.CreateFromJulian(60333.5).ToDateTimeString() // 2024-01-24 12:00:00
carbon.CreateFromJulian(60333.551563).ToDateTimeString() // 2024-01-24 13:14:15
```

### 中国农历
> 目前仅支持公元 `1900` 年至 `2100` 年的 `200` 年时间跨度

#### 将 `公历` 转换成 `农历`
```go
// 获取农历生肖
carbon.Parse("2020-08-05").Lunar().Animal() // 鼠
// 获取农历节日
carbon.Parse("2021-02-12").Lunar().Festival() // 春节

// 获取农历年份
carbon.Parse("2020-08-05").Lunar().Year() // 2020
// 获取农历月份
carbon.Parse("2020-08-05").Lunar().Month() // 6
// 获取农历闰月月份
carbon.Parse("2020-08-05").Lunar().LeapMonth() // 4
// 获取农历日期
carbon.Parse("2020-08-05").Lunar().Day() // 16

// 获取农历日期时间字符串
carbon.Parse("2020-08-05").Lunar().String() // 2020-06-16
fmt.Printf("%s", carbon.Parse("2020-08-05").Lunar()) // 2020-06-16
// 获取农历年字符串
carbon.Parse("2020-08-05").Lunar().ToYearString() // 二零二零
// 获取农历月字符串
carbon.Parse("2020-08-05").Lunar().ToMonthString() // 六月
// 获取农历闰月字符串
carbon.Parse("2020-04-23").Lunar().ToMonthString() // 闰四月
// 获取农历周字符串
carbon.Parse("2020-04-23").Lunar().ToWeekString() // 周四
// 获取农历天字符串
carbon.Parse("2020-08-05").Lunar().ToDayString() // 十六
// 获取农历日期字符串
carbon.Parse("2020-08-05").Lunar().ToDateString() // 二零二零年六月十六

```

#### 将 `农历` 转化成 `公历`
```go
// 将农历 二零二三年腊月十一 转化为 公历
carbon.CreateFromLunar(2023, 12, 11, false).ToDateString() // 2024-01-21
// 将农历 二零二三年二月十一 转化为 公历
carbon.CreateFromLunar(2023, 2, 11, false).ToDateString() // 2023-03-02
// 将农历 二零二三年闰二月十一 转化为 公历
carbon.CreateFromLunar(2023, 2, 11, true).ToDateString() // 2023-04-01
```

#### 日期判断
```go

// 是否是合法农历日期
carbon.Parse("0000-00-00").Lunar().IsValid() // false
carbon.Parse("2020-08-05").Lunar().IsValid() // true

// 是否是农历闰年
carbon.Parse("2020-08-05").Lunar().IsLeapYear() // true
// 是否是农历闰月
carbon.Parse("2020-08-05").Lunar().IsLeapMonth() // false

// 是否是鼠年
carbon.Parse("2020-08-05").Lunar().IsRatYear() // true
// 是否是牛年
carbon.Parse("2020-08-05").Lunar().IsOxYear() // false
// 是否是虎年
carbon.Parse("2020-08-05").Lunar().IsTigerYear() // false
// 是否是兔年
carbon.Parse("2020-08-05").Lunar().IsRabbitYear() // false
// 是否是龙年
carbon.Parse("2020-08-05").Lunar().IsDragonYear() // false
// 是否是蛇年
carbon.Parse("2020-08-05").Lunar().IsSnakeYear() // false
// 是否是马年
carbon.Parse("2020-08-05").Lunar().IsHorseYear() // false
// 是否是羊年
carbon.Parse("2020-08-05").Lunar().IsGoatYear() // false
// 是否是猴年
carbon.Parse("2020-08-05").Lunar().IsMonkeyYear() // false
// 是否是鸡年
carbon.Parse("2020-08-05").Lunar().IsRoosterYear() // false
// 是否是狗年
carbon.Parse("2020-08-05").Lunar().IsDogYear() // false
// 是否是猪年
carbon.Parse("2020-08-05").Lunar().IsPigYear() // false
```

### 波斯历/伊朗历

#### 将 `公历` 转换成 `波斯历`
```go
// 获取波斯历年份
carbon.Parse("2020-08-05").Persian().Year() // 1399
// 获取波斯历月份
carbon.Parse("2020-08-05").Persian().Month() // 5
// 获取波斯历日期
carbon.Parse("2020-08-05").Persian().Day() // 15

// 获取波斯历日期时间字符串
carbon.Parse("2020-08-05").Persian().String() // 1399-05-15
fmt.Printf("%s", carbon.Parse("2020-08-05").Persian()) // 1399-05-15

// 获取波斯历月字符串
carbon.Parse("2020-08-05").Persian().ToMonthString() // Mordad
carbon.Parse("2020-08-05").Persian().ToMonthString("en") // Mordad
carbon.Parse("2020-08-05").Persian().ToMonthString("fa") // مرداد

// 获取波斯历周字符串
carbon.Parse("2020-08-05").Persian().ToWeekString() // Chaharshanbeh
carbon.Parse("2020-08-05").Persian().ToWeekString("en") // Chaharshanbeh
carbon.Parse("2020-08-05").Persian().ToWeekString("fa") // چهارشنبه
```

#### 将 `波斯历` 转化成 `公历`
```go
carbon.CreateFromPersian(1, 1, 1).ToDateString() // 2016-03-20
carbon.CreateFromPersian(622, 1, 1).ToDateString() // 1243-03-21
carbon.CreateFromPersian(1395, 1, 1).ToDateString() // 2016-03-20
carbon.CreateFromPersian(9377, 1, 1).ToDateString() // 9998-03-19
```

#### 日期判断
```go
// 是否是合法的波斯历日期
carbon.CreateFromPersian(1, 1, 1).IsValid() // true
carbon.CreateFromPersian(622, 1, 1).IsValid() // true
carbon.CreateFromPersian(9377, 1, 1).IsValid() // true
carbon.CreateFromPersian(0, 0, 0, 0).IsValid() // false
carbon.CreateFromPersian(2024, 0, 1).IsValid() // false
carbon.CreateFromPersian(2024, 1, 0).IsValid() // false

// 是否是波斯历闰年
carbon.CreateFromPersian(1395, 1, 1).IsLeapYear() // true
carbon.CreateFromPersian(9377, 1, 1).IsLeapYear() // true
carbon.CreateFromPersian(622, 1, 1).IsLeapYear() // false
carbon.CreateFromPersian(9999, 1, 1).IsLeapYear() // false
```

### 希伯来历(犹太历)

#### 将 `公历` 转换成 `希伯来历`
```go
// 获取希伯来历年份
carbon.Parse("2024-01-01").Hebrew().Year() // 5784
// 获取希伯来历月份
carbon.Parse("2024-01-01").Hebrew().Month() // 10
// 获取希伯来历日期
carbon.Parse("2024-01-01").Hebrew().Day() // 20

// 获取希伯来历日期时间字符串
carbon.Parse("2024-01-01").Hebrew().String() // 5784-10-20
fmt.Printf("%s", carbon.Parse("2024-01-01").Hebrew()) // 5784-10-20

// 获取希伯来历月字符串
carbon.Parse("2020-08-05").Hebrew().ToMonthString() // Av
carbon.Parse("2020-08-05").Hebrew().ToMonthString("en") // Av
carbon.Parse("2020-08-05").Hebrew().ToMonthString("he") // אב

// 获取希伯来历周字符串
carbon.Parse("2020-08-05").Hebrew().ToWeekString() // Wednesday
carbon.Parse("2020-08-05").Hebrew().ToWeekString("en") // Wednesday
carbon.Parse("2020-08-05").Hebrew().ToWeekString("he") // רביעי
```

#### 将 `希伯来历` 转化成 `公历`
```go
carbon.CreateFromHebrew(5784, 10, 20).ToDateString() // 2023-12-17
carbon.CreateFromHebrew(5784, 5, 1).ToDateString() // 2024-07-21
carbon.CreateFromHebrew(5786, 7, 10).ToDateString() // 2025-09-18
```

#### 日期判断
```go
// 是否是合法的希伯来历日期
carbon.CreateFromHebrew(5780, 14, 1).IsValid() // false
carbon.CreateFromHebrew(5780, 10, 30).IsValid() // true

// 是否是希伯来历闰年
carbon.CreateFromHebrew(5784, 1, 1).IsLeapYear() // true
carbon.CreateFromHebrew(5788, 1, 1).IsLeapYear() // false
```


---

## 国际化

目前支持的本地化语言有(按照翻译先后顺序排列)

* [简体中文(zh-CN)](https://github.com/dromara/carbon/blob/master/lang/zh-CN.json "简体中文"): 由 [gouguoyin](https://github.com/gouguoyin "gouguoyin") 翻译
* [繁体中文(zh-TW)](https://github.com/dromara/carbon/blob/master/lang/zh-TW.json "繁体中文"): 由 [gouguoyin](https://github.com/gouguoyin "gouguoyin") 翻译
* [英语(en)](https://github.com/dromara/carbon/blob/master/lang/en.json "英语"): 由 [gouguoyin](https://github.com/gouguoyin "gouguoyin") 翻译
* [日语(ja)](https://github.com/dromara/carbon/blob/master/lang/ja.json "日语"): 由 [gouguoyin](https://github.com/gouguoyin "gouguoyin") 翻译
* [韩语(ko)](https://github.com/dromara/carbon/blob/master/lang/ko.json "韩语"): 由 [nannul](https://github.com/nannul "nannul") 翻译
* [德语(de)](https://github.com/dromara/carbon/blob/master/lang/de.json "德语"): 由 [benzammour](https://github.com/benzammour "benzammour") 翻译
* [西班牙语(es)](https://github.com/dromara/carbon/blob/master/lang/es.json "西班牙语"): 由 [hgisinger](https://github.com/hgisinger "hgisinger") 翻译
* [土耳其语(tr)](https://github.com/dromara/carbon/blob/master/lang/tr.json "土耳其语"): 由 [emresenyuva](https://github.com/emresenyuva "emresenyuva") 翻译
* [葡萄牙语(pt)](https://github.com/dromara/carbon/blob/master/lang/pt.json "葡萄牙语"): 由 [felipear89](https://github.com/felipear89 "felipear89") 翻译
* [俄罗斯语(ru)](https://github.com/dromara/carbon/blob/master/lang/ru.json "俄罗斯语"): 由 [zemlyak](https://github.com/zemlyak "zemlyak") 翻译
* [乌克兰语(uk)](https://github.com/dromara/carbon/blob/master/lang/uk.json "乌克兰语"): 由 [open-git](https://github.com/open-git "open-git") 翻译
* [罗马尼亚语(ro)](https://github.com/dromara/carbon/blob/master/lang/ro.json "罗马尼亚语"): 由 [DrOctavius](https://github.com/DrOctavius "DrOctavius") 翻译
* [印度尼西亚语(id)](https://github.com/dromara/carbon/blob/master/lang/id.json "印度尼西亚语"): 由 [justpoypoy](https://github.com/justpoypoy "justpoypoy") 翻译
* [意大利语(it)](https://github.com/dromara/carbon/blob/master/lang/it.json "意大利语"): 由 [nicoloHevelop](https://github.com/justpoypoy "nicoloHevelop") 翻译
* [马来西亚巴哈马语(ms-MY)](https://github.com/dromara/carbon/blob/master/lang/ms-MY.json "马来西亚巴哈马语"):
  由 [hollowaykeanho](https://github.com/hollowaykeanho "hollowaykeanho") 翻译
* [法语(fr)](https://github.com/dromara/carbon/blob/master/lang/fr.json "法语"): 由 [hollowaykeanho](https://github.com/hollowaykeanho "hollowaykeanho") 翻译
* [泰语(th)](https://github.com/dromara/carbon/blob/master/lang/th.json "泰语"): 由 [izcream](https://github.com/izcream "izcream") 翻译
* [瑞典语(se)](https://github.com/dromara/carbon/blob/master/lang/se.json "瑞典语"): 由 [jwanglof](https://github.com/jwanglof "jwanglof") 翻译
* [波斯语(fa)](https://github.com/dromara/carbon/blob/master/lang/fa.json "波斯语"): 由 [erfanMomeniii](https://github.com/ErfanMomeniii "ErfanMomeniii") 翻译
* [波兰语(nl)](https://github.com/dromara/carbon/blob/master/lang/nl.json "波兰语"): 由 [RemcoE33](https://github.com/RemcoE33 "RemcoE33") 翻译
* [越南语(vi)](https://github.com/dromara/carbon/blob/master/lang/vi.json "越南语"): 由 [culy247](https://github.com/culy247 "culy247") 翻译
* [印地语(hi)](https://github.com/dromara/carbon/blob/master/lang/hi.json "印地语"): 由 [chauhan17nitin](https://github.com/chauhan17nitin "chauhan17nitin") 翻译
* [波兰语(pl)](https://github.com/dromara/carbon/blob/master/lang/pl.json "波兰语"): 由 [gouguoyin](https://github.com/gouguoyin "gouguoyin") 翻译
* [保加利亚语(bg)](https://github.com/dromara/carbon/blob/master/lang/bg.json "保加利亚语"): 由 [yuksbg](https://github.com/yuksbg "yuksbg") 翻译
* [阿拉伯语(ar)](https://github.com/dromara/carbon/blob/master/lang/bg.json "阿拉伯语"): 由 [zumoshi](https://github.com/zumoshi "zumoshi") 翻译
* [匈牙利语(hu)](https://github.com/dromara/carbon/blob/master/lang/hu.json "匈牙利语"): 由 [kenlas](https://github.com/kenlas "kenlas") 翻译
* [丹麦语(da)](https://github.com/dromara/carbon/blob/master/lang/da.json "丹麦语"): 由 [Munk91](https://github.com/Munk91 "Munk91") 翻译
* [挪威语(nb)](https://github.com/dromara/carbon/blob/master/lang/nb.json "挪威语"): 由 [bendikrb](https://github.com/bendikrb "bendikrb") 翻译
* [希腊语(el)](https://github.com/dromara/carbon/blob/master/lang/el.json "希腊语"): 由 [cursor](https://cursor.com "cursor") 翻译
* [芬兰语(fi)](https://github.com/dromara/carbon/blob/master/lang/fi.json "芬兰语"): 由 [cursor](https://cursor.com "cursor") 翻译
* [缅甸语(my)](https://github.com/dromara/carbon/blob/master/lang/my.json "缅甸语"): 由 [cursor](https://cursor.com "cursor") 翻译
* [南非语(af)](https://github.com/dromara/carbon/blob/master/lang/af.json "南非语"): 由 [cursor](https://cursor.com "cursor") 翻译
* [蒙古语(mn)](https://github.com/dromara/carbon/blob/master/lang/mn.json "蒙古语"): 由 [cursor](https://cursor.com "cursor") 翻译

[如何为 carbon 添加新的本地化语言支持](../appendix/contribution-guide.md)

目前支持的方法有

* `Constellation()`：获取星座，如 `白羊座`
* `Season()`：获取季节，如 `夏季`
* `DiffForHumans()`：获取对人类友好的可读格式时间差，如 `一小时前`
* `ToMonthString()`：输出完整月份字符串，如 `一月`
* `ToShortMonthString()`：输出缩写月份字符串，如 `1月`
* `ToWeekString()`：输出完整星期字符串，如 `星期一`
* `ToShortWeekString()`：输出缩写星期字符串，如 `周一`

### 设置区域

```go
lang := carbon.NewLanguage()
lang.SetLocale("zh-CN")

carbon.SetTestNow(carbon.Parse("2020-08-05 13:14:15"))
now := carbon.Now().SetLanguage(lang)

now.AddHours(1).DiffForHumans() // 1 小时后
now.AddHours(1).ToMonthString() // 八月
now.AddHours(1).ToShortMonthString() // 8月
now.AddHours(1).ToWeekString() // 星期二
now.AddHours(1).ToShortWeekString() // 周二
now.AddHours(1).Constellation() // 狮子座
now.AddHours(1).Season() // 夏季
```

### 重写部分翻译资源
> 其余仍然按照指定的 `locale` 文件内容翻译
```go
lang := carbon.NewLanguage()

resources := map[string]string {
	"hour": "%dh",
}
lang.SetLocale("en").SetResources(resources)

carbon.SetTestNow(carbon.Parse("2020-08-05 13:14:15"))
now := carbon.Now().SetLanguage(lang)

now.AddYears(1).DiffForHumans() // 1 year from now
now.AddHours(1).DiffForHumans() // 1h from now
now.ToMonthString() // August
now.ToShortMonthString() // Aug
now.ToWeekString() // Tuesday
now.ToShortWeekString() // Tue
now.Constellation() // Leo
now.Season() // Summer
```

### 重写全部翻译资源
> 无需指定 `locale`
```go
lang := carbon.NewLanguage()
resources := map[string]string {
  "months": "january|february|march|april|may|june|july|august|september|october|november|december",
  "short_months": "jan|feb|mar|apr|may|jun|jul|aug|sep|oct|nov|dec",
  "weeks": "sunday|monday|tuesday|wednesday|thursday|friday|saturday",
  "short_weeks": "sun|mon|tue|wed|thu|fri|sat",
  "seasons": "spring|summer|autumn|winter",
  "constellations": "aries|taurus|gemini|cancer|leo|virgo|libra|scorpio|sagittarius|capricornus|aquarius|pisce",
  "year": "1 yr|%d yrs",
  "month": "1 mo|%d mos",
  "week": "%dw",
  "day": "%dd",
  "hour": "%dh",
  "minute": "%dm",
  "second": "%ds",
  "now": "just now",
  "ago": "%s ago",
  "from_now": "in %s",
  "before": "%s before",
  "after": "%s after",
}
lang.SetResources(resources)

carbon.SetTestNow(carbon.Parse("2020-08-05 13:14:15"))
now := carbon.Now().SetLanguage(lang)

now.AddYears(1).DiffForHumans() // in 1 yr
now.AddHours(1).DiffForHumans() // in 1h
now.ToMonthString() // august
now.ToShortMonthString() // aug
now.ToWeekString() // tuesday
now.ToShortWeekString() // tue
now.Constellation() // leo
now.Season() // summer
```


---

## JSON

### 内置字段类型

```go
type User struct {
  Date      *carbon.Date      `json:"date"`
  DateMilli *carbon.DateMilli `json:"date_milli"`
  DateMicro *carbon.DateMicro `json:"date_micro"`
  DateNano  *carbon.DateNano  `json:"date_nano"`
  
  Time      *carbon.Time      `json:"time"`
  TimeMilli *carbon.TimeMilli `json:"time_milli"`
  TimeMicro *carbon.TimeMicro `json:"time_micro"`
  TimeNano  *carbon.TimeNano  `json:"time_nano"`
  
  DateTime      *carbon.DateTime      `json:"date_time"`
  DateTimeMilli *carbon.DateTimeMilli `json:"date_time_milli"`
  DateTimeMicro *carbon.DateTimeMicro `json:"date_time_micro"`
  DateTimeNano  *carbon.DateTimeNano  `json:"date_time_nano"`
  
  Timestamp      *carbon.Timestamp      `json:"timestamp"`
  TimestampMilli *carbon.TimestampMilli `json:"timestamp_milli"`
  TimestampMicro *carbon.TimestampMicro `json:"timestamp_micro"`
  TimestampNano  *carbon.TimestampNano  `json:"timestamp_nano"`

  CreatedAt *carbon.DateTime `json:"created_at"`
  UpdatedAt *carbon.DateTime `json:"updated_at"`
  DeletedAt *carbon.Timestamp `json:"deleted_at"`
}

var user User

c := carbon.Parse("2020-08-05 13:14:15.999999999")

user.Date      = carbon.NewDate(c)
user.DateMilli = carbon.NewDateMilli(c)
user.DateMicro = carbon.NewDateMicro(c)
user.DateNano  = carbon.NewDateNano(c)

user.Time      = carbon.NewTime(c)
user.TimeMilli = carbon.NewTimeMilli(c)
user.TimeMicro = carbon.NewTimeMicro(c)
user.TimeNano  = carbon.NewTimeNano(c)

user.DateTime      = carbon.NewDateTime(c)
user.DateTimeMilli = carbon.NewDateTimeMilli(c)
user.DateTimeMicro = carbon.NewDateTimeMicro(c)
user.DateTimeNano  = carbon.NewDateTimeNano(c)

user.Timestamp      = carbon.NewTimestamp(c)
user.TimestampMilli = carbon.NewTimestampMilli(c)
user.TimestampMicro = carbon.NewTimestampMicro(c)
user.TimestampNano  = carbon.NewTimestampNano(c)

user.CreatedAt = carbon.NewDateTime(c)
user.UpdatedAt = carbon.NewDateTime(c)
user.DeletedAt = carbon.NewTimestamp(c)

data, err := json.Marshal(&user)
if err != nil {
  // 错误处理
  log.Fatal(err)
}
fmt.Printf("%s\n", data)
// 输出
{
  "date": "2020-08-05",
  "date_milli": "2020-08-05.999",
  "date_micro": "2020-08-05.999999",
  "date_nano": "2020-08-05.999999999",
  "time": "13:14:15",
  "time_milli": "13:14:15.999",
  "time_micro": "13:14:15.999999",
  "time_nano": "13:14:15.999999999",
  "date_time": "2020-08-05 13:14:15",
  "date_time_milli": "2020-08-05 13:14:15.999",
  "date_time_micro": "2020-08-05 13:14:15.999999",
  "date_time_nano": "2020-08-05 13:14:15.999999999",
  "timestamp": 1596633255,
  "timestamp_milli": 1596633255999,
  "timestamp_micro": 1596633255999999,
  "timestamp_nano": 1596633255999999999,
  "created_at": "2020-08-05 13:14:15",
  "updated_at": "2020-08-05 13:14:15",
  "deleted_at": 1596633255
}

var person User
err := json.Unmarshal(data, &person)
if err != nil {
  // 错误处理
  log.Fatal(err)
}

fmt.Printf("person: %+v\n", person)
// 输出
person: {Date:2020-08-05 DateMilli:2020-08-05.999 DateMicro:2020-08-05.999999 DateNano:2020-08-05.999999999 Time:13:14:15 TimeMilli:13:14:15.999 TimeMicro:13:14:15.999999 TimeNano:13:14:15.999999999 DateTime:2020-08-05 13:14:15 DateTimeMilli:2020-08-05 13:14:15.999 DateTimeMicro:2020-08-05 13:14:15.999999 DateTimeNano:2020-08-05 13:14:15.999999999 Timestamp:1596633255 TimestampMilli:1596633255999 TimestampMicro:1596633255999999 TimestampNano:1596633255999999999 CreatedAt:2020-08-05 13:14:15 UpdatedAt:2020-08-05 13:14:15 DeletedAt:1596633255}
```

### 自定义字段类型
```go
type RFC3339Type string
func (RFC3339Type) Layout() string {
  return carbon.RFC3339Layout
}

type ISO8601Type string
func (ISO8601Type) Format() string {
  return carbon.ISO8601Format
}

type User struct {
  Customer1 *carbon.LayoutType[RFC3339Type] `json:"customer1"`
  Customer2 *carbon.FormatType[ISO8601Type] `json:"customer2"`
}

var user User

c := carbon.Parse("2020-08-05 13:14:15")

user.Customer1 = carbon.NewLayoutType[RFC3339Type](c)
user.Customer2 = carbon.NewFormatType[ISO8601Type](c)

data, err := json.Marshal(&user)
if err != nil {
  // 错误处理 
  //log.Fatal(err)
}
fmt.Printf("%s\n", data)
// 输出
{"customer1":"2020-08-05T13:14:15Z", "customer2":"2020-08-05T13:14:15+00:00"}

var person User
err := json.Unmarshal(data, &person)
if err != nil {
  // 错误处理
  log.Fatal(err)
}

fmt.Printf("person: %+v\n", person)
// 输出
person: {Customer1:2020-08-05T13:14:15Z Customer2:2020-08-05T13:14:15+00:00}
```


---

## 设置全局默认值
默认时区是 `UTC`, 语言环境是 `英语`，一周开始日期是 `周一`，周末是 `周六`和 `周日`。

### 设置单个默认值
```go
carbon.SetLayout(carbon.DateTimeLayout)
carbon.SetTimezone(carbon.UTC)
carbon.SetLocale("en")
carbon.SetWeekStartsAt(carbon.Monday)
carbon.SetWeekendDays([]carbon.Weekday{carbon.Saturday, carbon.Sunday,})
```

### 设置多个默认值
```go
carbon.SetDefault(carbon.Default{
  Layout: carbon.DateTimeLayout,
  Timezone: carbon.UTC,
  Locale: "en",
  WeekStartsAt: carbon.Monday,
  WeekendDays: []carbon.Weekday{carbon.Saturday, carbon.Sunday,},
})
```

### 重置默认值
```go
carbon.ResetDefault()
```
这在测试中特别有用，确保一个测试中的更改不会影响其他测试。


---

## 测试
支持冻结固定时间，将任意时间设置为`当前时间`，使后续操作基于此模拟时间运行，而非真实系统时间，便于单元测试

### 设置测试时间
```go
now := carbon.Parse("2020-08-05")
carbon.SetTestNow(now)

carbon.Now().ToDateString() // 2020-08-05
carbon.Yesterday().ToDateString() // 2020-08-04
carbon.Tomorrow().ToDateString() // 2020-08-05
carbon.Now().DiffForHumans() // just now
carbon.Yesterday().DiffForHumans() // 1 day ago
carbon.Tomorrow().DiffForHumans() // 1 day from now
carbon.Parse("2020-10-05").DiffForHumans() // 2 months from now
now.DiffForHumans(carbon.Parse("2020-10-05")) // 2 months before
```

### 是否是测试时间
```go
carbon.IsTestNow() 
```

### 清除测试时间
```go
carbon.ClearTestNow()
```


---

## 错误处理
如果出现多个错误，则只返回第一个错误，访问 <a href="https://github.com/dromara/carbon/blob/master/errors.go" target="_blank" rel="noreferrer">errors.go</a> 查看所有可能出现的错误

```go
c1 := carbon.Parse("xxx")
if c1.HasError() {
  // 错误处理...
  log.Fatal(c1.Error)
}
// 输出
failed to parse "xxx" as carbon

c2 := carbon.Parse("2020-08-05").SetTimezone("xxx")
if c2.HasError() {
  // 错误处理...
  log.Fatal(c2.Error)
}
// 输出
invalid timezone "xxx", please see the file "$GOROOT/lib/time/zoneinfo.zip" for all valid timezones

c3 := carbon.Parse("xxx").SetTimezone("xxx")
if c3.HasError() {
  // 错误处理...
  log.Fatal(c3.Error)
}
// 输出
failed to parse "xxx" as carbon
```


---

## 常见问题

1、v2.5.x 和 v2.6.x 版本有什么区别？
> `v2.5.x` 及以下版本是`值`传递， `v2.6.x` 及以上版本是`指针`传递，强烈建议使用 `v2.6.x` 及以上版本。

2、window 系统部署时时区报错

> `window` 系统如果没有安装 `golang` 环境，部署时会报 `GOROOT/lib/time/zoneinfo.zip: no such file or directory` 异常，原因是由于
> `window` 系统没有内置时区文件，只需要手动下载并指定 `zoneinfo.zip` 路径即可，如 `go/lib/time/zoneinfo.zip`

```go
os.Setenv("ZONEINFO", "./go/lib/time/zoneinfo.zip")
```

3、docker 容器部署时时区报错

> `docker` 容器如果没有安装 `golang` 环境，部署时会报 `open /usr/local/go/lib/time/zoneinfo.zip: no such file or directory`
> 异常，只需要把 `zoneinfo.zip` 复制到容器中即可，即在 `Dockerfile` 中加入

```go
COPY ./zoneinfo.zip /usr/local/go /lib/time/zoneinfo.zip
```


---

## 内置常量

### 版本常量
```go
carbon.Version // 2.6.15
```

### 时区常量
```go
carbon.Local // Local
carbon.UTC   // UTC

carbon.CET  // CET
carbon.EET  // EET
carbon.EST  // EST
carbon.GMT  // GMT
carbon.MET  // MET
carbon.MST  // MST
carbon.UCT  // MST
carbon.WET  // WET
carbon.Zulu // Zulu

carbon.Cuba      // Cuba
carbon.Egypt     // Egypt
carbon.Eire      // Eire
carbon.Greenwich // Greenwich
carbon.Iceland   // Iceland
carbon.Iran      // Iran
carbon.Israel    // Israel
carbon.Jamaica   // Jamaica
carbon.Japan     // Japan
carbon.Libya     // Libya
carbon.Poland    // Poland
carbon.Portugal  // Portugal
carbon.PRC       // PRC
carbon.Singapore // Singapore
carbon.Turkey    // Turkey

carbon.Shanghai   // Asia/Shanghai
carbon.Chongqing  // Asia/Chongqing
carbon.Harbin     // Asia/Harbin
carbon.Urumqi     // Asia/Urumqi
carbon.HongKong   // Asia/Hong_Kong
carbon.Macao      // Asia/Macao
carbon.Taipei     // Asia/Taipei
carbon.Tokyo      // Asia/Tokyo
carbon.HoChiMinh  // Asia/Ho_Chi_Minh
carbon.Hanoi      // Asia/Hanoi
carbon.Saigon     // Asia/Saigon
carbon.Seoul      // Asia/Seoul
carbon.Pyongyang  // Asia/Pyongyang
carbon.Bangkok    // Asia/Bangkok
carbon.Dubai      // Asia/Dubai
carbon.Qatar      // Asia/Qatar
carbon.Bangalore  // Asia/Bangalore
carbon.Kolkata    // Asia/Kolkata
carbon.Mumbai     // Asia/Mumbai
carbon.MexicoCity // America/Mexico_City
carbon.NewYork    // America/New_York
carbon.LosAngeles // America/Los_Angeles
carbon.Chicago    // America/Chicago
carbon.SaoPaulo   // America/Sao_Paulo
carbon.Moscow     // Europe/Moscow
carbon.London     // Europe/London
carbon.Berlin     // Europe/Berlin
carbon.Paris      // Europe/Paris
carbon.Rome       // Europe/Rome
carbon.Sydney     // Australia/Sydney
carbon.Melbourne  // Australia/Melbourne
carbon.Darwin     // Australia/Darwin
```

### 月份常量
```go
carbon.January   // time.January
carbon.February  // time.February
carbon.March     // time.March
carbon.April     // time.April
carbon.May       // time.May
carbon.June      // time.June
carbon.July      // time.July
carbon.August    // time.August
carbon.September // time.September
carbon.October   // time.October
carbon.November  // time.November
carbon.December  // time.December
```

### 季节常量
```go
carbon.Spring // Spring
carbon.Summer // Summer
carbon.Autumn // Autumn
carbon.Winter // Winter
```

### 星座常量
```go
carbon.Aries       // Aries
carbon.Taurus      // Taurus
carbon.Gemini      // Gemini
carbon.Cancer      // Cancer
carbon.Leo         // Leo
carbon.Virgo       // Virgo
carbon.Libra       // Libra
carbon.Scorpio     // Scorpio
carbon.Sagittarius // Sagittarius
carbon.Capricorn   // Capricorn
carbon.Aquarius    // Aquarius
carbon.Pisces      // Pisces
```

### 星期常量
```go
carbon.Monday    // time.Monday
carbon.Tuesday   // time.Tuesday
carbon.Wednesday // time.Wednesday
carbon.Thursday  // time.Thursday
carbon.Friday    // time.Friday
carbon.Saturday  // time.Saturday
carbon.Sunday    // time.Sunday
```

### 数字常量
```go
carbon.YearsPerMillennium // 1000
carbon.YearsPerCentury    // 100
carbon.YearsPerDecade     // 10
carbon.QuartersPerYear    // 4
carbon.MonthsPerYear      // 12
carbon.MonthsPerQuarter   // 3
carbon.WeeksPerNormalYear // 52
carbon.WeeksPerLongYear   // 53
carbon.WeeksPerMonth      // 4
carbon.DaysPerLeapYear    // 366
carbon.DaysPerNormalYear  // 365
carbon.DaysPerWeek        // 7
carbon.HoursPerWeek       // 168
carbon.HoursPerDay        // 24
carbon.MinutesPerDay      // 1440
carbon.MinutesPerHour     // 60
carbon.SecondsPerWeek     // 604800
carbon.SecondsPerDay      // 86400
carbon.SecondsPerHour     // 3600
carbon.SecondsPerMinute   // 60

carbon.EpochYear     // 1970
carbon.MaxYear       // 9999
carbon.MinYear       // 1
carbon.MaxMonth      // 12
carbon.MinMonth      // 1
carbon.MaxDay        // 31
carbon.MinDay        // 1
carbon.MaxHour       // 23
carbon.MinHour       // 0
carbon.MaxMinute     // 59
carbon.MinMinute     // 0
carbon.MaxSecond     // 59
carbon.MinSecond     // 0
carbon.MaxNanosecond // 999999999
carbon.MinNanosecond // 0
```

### 布局常量
```go
carbon.AtomLayout     // 2006-01-02T15:04:05Z07:00"
carbon.ANSICLayout    // Mon Jan _2 15:04:05 2006
carbon.CookieLayout   // Monday, 02-Jan-2006 15:04:05 MST
carbon.KitchenLayout  // 3:04PM
carbon.RssLayout      // Mon, 02 Jan 2006 15:04:05 -0700
carbon.RubyDateLayout // Mon Jan 02 15:04:05 -0700 2006
carbon.UnixDateLayout // Mon Jan _2 15:04:05 MST 2006
carbon.W3cLayout      // 2006-01-02T15:04:05Z07:00
carbon.HttpLayout     // Mon, 02 Jan 2006 15:04:05 GMT

carbon.RFC1036Layout      // Mon, 02 Jan 06 15:04:05 -0700
carbon.RFC1123Layout      // Mon, 02 Jan 2006 15:04:05 MST
carbon.RFC1123ZLayout     // Mon, 02 Jan 2006 15:04:05 -0700
carbon.RFC2822Layout      // Mon, 02 Jan 2006 15:04:05 -0700
carbon.RFC3339Layout      // 2006-01-02T15:04:05Z07:00
carbon.RFC3339MilliLayout // 2006-01-02T15:04:05.999Z07:00
carbon.RFC3339MicroLayout // 2006-01-02T15:04:05.999999Z07:00
carbon.RFC3339NanoLayout  // 2006-01-02T15:04:05.999999999Z07:00
carbon.RFC7231Layout      // Mon, 02 Jan 2006 15:04:05 MST
carbon.RFC822Layout       // 02 Jan 06 15:04 MST
carbon.RFC822ZLayout      // 02 Jan 06 15:04 -0700
carbon.RFC850Layout       // Monday, 02-Jan-06 15:04:05 MST

carbon.ISO8601Layout      // 2006-01-02T15:04:05-07:00
carbon.ISO8601MilliLayout // 2006-01-02T15:04:05.999-07:00
carbon.ISO8601MicroLayout // 2006-01-02T15:04:05.999999-07:00
carbon.ISO8601NanoLayout  // 2006-01-02T15:04:05.999999999-07:00

carbon.ISO8601ZuluLayout      // 2006-01-02T15:04:05Z
carbon.ISO8601ZuluMilliLayout // 2006-01-02T15:04:05.999Z
carbon.ISO8601ZuluMicroLayout // 2006-01-02T15:04:05.999999Z
carbon.ISO8601ZuluNanoLayout  // 2006-01-02T15:04:05.999999999Z

carbon.FormattedDateLayout    // Jan 2, 2006
carbon.FormattedDayDateLayout // Mon, Jan 2, 2006

carbon.DayDateTimeLayout        // Mon, Jan 2, 2006 3:04 PM
carbon.DateTimeLayout           // 2006-01-02 15:04:05
carbon.DateTimeMilliLayout      // 2006-01-02 15:04:05.999
carbon.DateTimeMicroLayout      // 2006-01-02 15:04:05.999999
carbon.DateTimeNanoLayout       // 2006-01-02 15:04:05.999999999
carbon.ShortDateTimeLayout      // 20060102150405
carbon.ShortDateTimeMilliLayout // 20060102150405.999
carbon.ShortDateTimeMicroLayout // 20060102150405.999999
carbon.ShortDateTimeNanoLayout  // 20060102150405.999999999

carbon.DateLayout           // 2006-01-02
carbon.DateMilliLayout      // 2006-01-02.999
carbon.DateMicroLayout      // 2006-01-02.999999
carbon.DateNanoLayout       // 2006-01-02.999999999
carbon.ShortDateLayout      // 20060102
carbon.ShortDateMilliLayout // 20060102.999
carbon.ShortDateMicroLayout // 20060102.999999
carbon.ShortDateNanoLayout  // 20060102.999999999

carbon.TimeLayout           // 15:04:05
carbon.TimeMilliLayout      // 15:04:05.999
carbon.TimeMicroLayout      // 15:04:05.999999
carbon.TimeNanoLayout       // 15:04:05.999999999
carbon.ShortTimeLayout      // 150405
carbon.ShortTimeMilliLayout // 150405.999
carbon.ShortTimeMicroLayout // 150405.999999
carbon.ShortTimeNanoLayout  // 150405.999999999

carbon.TimestampLayout      // unix
carbon.TimestampMilliLayout // unixMilli
carbon.TimestampMicroLayout // unixMicro
carbon.TimestampNanoLayout  // unixNano
```

### 格式常量
```go
carbon.AtomFormat     // Y-m-d\\TH:i:sR
carbon.ANSICFormat    // D M  j H:i:s Y
carbon.CookieFormat   // l, d-M-Y H:i:s Z
carbon.KitchenFormat  // g:iA
carbon.RssFormat      // D, d M Y H:i:s O
carbon.RubyDateFormat // D M d H:i:s O Y
carbon.UnixDateFormat // D M  j H:i:s Z Y
carbon.W3cFormat      // Y-m-d\\TH:i:sR
carbon.HttpFormat     // D, d M Y H:i:s GMT

carbon.RFC1036Format      // D, d M y H:i:s O
carbon.RFC1123Format      // D, d M Y H:i:s Z
carbon.RFC1123ZFormat     // D, d M Y H:i:s O
carbon.RFC2822Format      // D, d M Y H:i:s O
carbon.RFC3339Format      // Y-m-d\\TH:i:sR
carbon.RFC3339MilliFormat // Y-m-d\\TH:i:s.uR
carbon.RFC3339MicroFormat // Y-m-d\\TH:i:s.vR
carbon.RFC3339NanoFormat  // Y-m-d\\TH:i:s.xR
carbon.RFC7231Format      // D, d M Y H:i:s Z
carbon.RFC822Format       // d M y H:i Z
carbon.RFC822ZFormat      // d M y H:i O
carbon.RFC850Format       // l, d-M-y H:i:s Z

carbon.ISO8601Format      // Y-m-d\\TH:i:sP
carbon.ISO8601MilliFormat // Y-m-d\\TH:i:s.uP
carbon.ISO8601MicroFormat // Y-m-d\\TH:i:s.vP
carbon.ISO8601NanoFormat  // Y-m-d\\TH:i:s.xP

carbon.ISO8601ZuluFormat      // Y-m-d\\TH:i:s\\Z
carbon.ISO8601ZuluMilliFormat // Y-m-d\\TH:i:s.u\\Z
carbon.ISO8601ZuluMicroFormat // Y-m-d\\TH:i:s.v\\Z
carbon.ISO8601ZuluNanoFormat  // Y-m-d\\TH:i:s.x\\Z

carbon.FormattedDateFormat    // M j, Y
carbon.FormattedDayDateFormat // D, M j, Y

carbon.DayDateTimeFormat        // D, M j, Y g:i A
carbon.DateTimeFormat           // Y-m-d H:i:s
carbon.DateTimeMilliFormat      // Y-m-d H:i:s.u
carbon.DateTimeMicroFormat      // Y-m-d H:i:s.v
carbon.DateTimeNanoFormat       // Y-m-d H:i:s.x
carbon.ShortDateTimeFormat      // YmdHis
carbon.ShortDateTimeMilliFormat // YmdHis.u
carbon.ShortDateTimeMicroFormat // YmdHis.v
carbon.ShortDateTimeNanoFormat  // YmdHis.x

carbon.DateFormat           // Y-m-d
carbon.DateMilliFormat      // Y-m-d.u
carbon.DateMicroFormat      // Y-m-d.v
carbon.DateNanoFormat       // Y-m-d.x
carbon.ShortDateFormat      // Ymd
carbon.ShortDateMilliFormat // Ymd.u
carbon.ShortDateMicroFormat // Ymd.v
carbon.ShortDateNanoFormat  // Ymd.x

carbon.TimeFormat           // H:i:s
carbon.TimeMilliFormat      // H:i:s.u
carbon.TimeMicroFormat      // H:i:s.v
carbon.TimeNanoFormat       // H:i:s.x
carbon.ShortTimeFormat      // His
carbon.ShortTimeMilliFormat // His.u
carbon.ShortTimeMicroFormat // His.v
carbon.ShortTimeNanoFormat  // His.x

carbon.TimestampFormat      // S
carbon.TimestampMilliFormat // U
carbon.TimestampMicroFormat // V
carbon.TimestampNanoFormat  // X
```


---

## 格式符号
`Format`和 `ParseByFormat`方法并不是完全可逆的，`Format` 方法支持以下所有符号，`ParseByFormat` 方法不支持 `K`, `W`, `N` `L`, `w`, `t`, `o`, `q`, `c`，因为这些符号是自定义的，在标准时间库中没有相应的符号。

| 符号 |             描述             | 长度  |        范围        |         示例          |
|:--:|:--------------------------:|:---:|:----------------:|:-------------------:|
| d  |        月份中的第几天，有前导零        |  2  |      01-31       |         02          |
| D  |         缩写单词表示的周几          |  3  |     Mon-Sun      |         Mon         |
| j  |       月份中的第几天，没有前导零        |  -  |       1-31       |          2          |
| K  |  第几天的英文缩写后缀，一般和 `j` 配合使用   |  2  |   st/nd/rd/th    |         th          |
| l  |         完整单词表示的周几          |  -  |  Monday-Sunday   |       Monday        |
| F  |         完整单词表示的月份          |  -  | January-December |       January       |
| m  |        数字表示的月份，有前导零        |  2  |      01-12       |         01          |
| M  |         缩写单词表示的月份          |  3  |     Jan-Dec      |         Jan         |
| n  |       数字表示的月份，没有前导零        |  -  |       1-12       |          1          |
| Y  |        4 位数字完整表示的年份        |  4  |    0000-9999     |        2006         |
| y  |         2 位数字表示的年份         |  2  |      00-99       |         06          |
| a  |         小写的上午和下午标识         |  2  |      am/pm       |         pm          |
| A  |         大写的上午和下午标识         |  2  |      AM/PM       |         PM          |
| g  |         小时，12 小时格式         |  -  |       1-12       |          3          |
| G  |         小时，24 小时格式         |  -  |       0-23       |         15          |
| h  |         小时，12 小时格式         |  2  |      00-11       |         03          |
| H  |         小时，24 小时格式         |  2  |      00-23       |         15          |
| i  |             分钟             |  2  |      01-59       |         04          |
| s  |             秒数             |  2  |      01-59       |         05          |
| O  |       与格林威治时间相差的小时数        |  -  |        -         |        -0700        |
| P  | 与格林威治时间相差的小时数，小时和分钟之间有冒号分隔 |  -  |        -         |       -07:00        |
| Z  |            时区名字            |  -  |        -         |         CST         |
| W  |          年份中的第几周           | 1-2 |       1-52       |          1          |
| N  |          星期中的第几天           |  1  |       1-7        |          2          |
| L  |    是否为闰年，如果是闰年为 1，否则为 0    |  1  |       0-1        |          0          |
| S  |           秒精度时间戳           |  -  |        -         |     1596604455      |
| U  |          毫秒精度时间戳           |  -  |        -         |    1596604455666    |
| V  |          微秒精度时间戳           |  -  |        -         |  1596604455666666   |
| X  |          纳秒精度时间戳           |  -  |        -         | 1596604455666666666 |
| u  |             毫秒             |  -  |      1-999       |         999         |
| v  |             微秒             |  -  |     1-999999     |       999999        |
| x  |             纳秒             |  -  |   1-999999999    |      999999999      |
| w  |          数字表示的周几           |  1  |       0-6        |          1          |
| t  |          月份中的总天数           |  2  |      28-31       |         31          |
| z  |            时区位置            |  -  |        -         |    Asia/Shanghai    |
| o  |           时区偏移量            |  -  |        -         |        28800        |
| q  |            当前季节            |  1  |       1-4        |          1          |
| c  |           当前世纪数            |  -  |       0-99       |         21          |
]()


---

## 语言代码对照表

根据 `ISO639-1` 标准，以下是常用的双字母语言代码对照表：

| 语言代码 | 语言名称                | 中文名称       | 本地名称                 |
|------|---------------------|------------|----------------------|
| aa   | Afar                | 阿法尔语       | Afaraf               |
| ab   | Abkhaz              | 阿布哈兹语      | аҧсуа бызшәа         |
| ae   | Avestan             | 阿维斯陀语      | avesta               |
| af   | Afrikaans           | 南非语        | Afrikaans            |
| ak   | Akan                | 阿坎语        | Akan                 |
| am   | Amharic             | 阿姆哈拉语      | አማርኛ                 |
| an   | Aragonese           | 阿拉贡语       | aragonés             |
| ar   | Arabic              | 阿拉伯语       | العربية              |
| as   | Assamese            | 阿萨姆语       | অসমীয়া              |
| av   | Avaric              | 阿瓦尔语       | авар мацӀ            |
| ay   | Aymara              | 艾马拉语       | aymar aru            |
| az   | Azerbaijani         | 阿塞拜疆语      | Azərbaycan dili      |
| ba   | Bashkir             | 巴什基尔语      | башҡорт теле         |
| be   | Belarusian          | 白俄罗斯语      | Беларуская мова      |
| bg   | Bulgarian           | 保加利亚语      | Български език       |
| bh   | Bihari              | 比哈尔语       | भोजपुरी              |
| bi   | Bislama             | 比斯拉马语      | Bislama              |
| bm   | Bambara             | 班巴拉语       | bamanankan           |
| bn   | Bengali             | 孟加拉语       | বাংলা                |
| bo   | Tibetan             | 藏语         | བོད་ཡིག              |
| br   | Breton              | 布列塔尼语      | brezhoneg            |
| bs   | Bosnian             | 波斯尼亚语      | Bosanski jezik       |
| ca   | Catalan             | 加泰罗尼亚语     | Català               |
| ce   | Chechen             | 车臣语        | нохчийн мотт         |
| ch   | Chamorro            | 查莫罗语       | Chamoru              |
| co   | Corsican            | 科西嘉语       | corsu                |
| cr   | Cree                | 克里语        | ᓀᐦᐃᔭᐍᐏᐣ              |
| cs   | Czech               | 捷克语        | čeština              |
| cu   | Old Church Slavonic | 古教会斯拉夫语    | ѩзыкъ словѣньскъ     |
| cv   | Chuvash             | 楚瓦什语       | чӑваш чӗлхи          |
| cy   | Welsh               | 威尔士语       | Cymraeg              |
| da   | Danish              | 丹麦语        | dansk                |
| de   | German              | 德语         | Deutsch              |
| dv   | Divehi              | 迪维西语       | Dhivehi              |
| dz   | Dzongkha            | 不丹语        | རྫོང་ཁ               |
| ee   | Ewe                 | 埃维语        | Eʋegbe               |
| el   | Greek               | 现代希腊语      | Ελληνικά             |
| en   | English             | 英语         | English              |
| eo   | Esperanto           | 世界语        | Esperanto            |
| es   | Spanish             | 西班牙语       | Español              |
| et   | Estonian            | 爱沙尼亚语      | eesti                |
| eu   | Basque              | 巴斯克语       | euskara              |
| fa   | Persian             | 波斯语        | فارسی                |
| ff   | Fula                | 富拉语        | Fulfulde             |
| fi   | Finnish             | 芬兰语        | suomi                |
| fj   | Fijian              | 斐济语        | Vakaviti             |
| fo   | Faroese             | 法罗语        | føroyskt             |
| fr   | French              | 法语         | Français             |
| fy   | Western Frisian     | 弗里西亚语      | Frysk                |
| ga   | Irish               | 爱尔兰语       | Gaeilge              |
| gd   | Scottish Gaelic     | 苏格兰盖尔语     | Gàidhlig             |
| gl   | Galician            | 加利西亚语      | galego               |
| gn   | Guarani             | 瓜拉尼语       | avañe'ẽ              |
| gu   | Gujarati            | 古吉拉特语      | ગુજરાતી              |
| gv   | Manx                | 马恩岛语       | Gaelg                |
| ha   | Hausa               | 豪萨语        | هَوُسَ               |
| he   | Hebrew              | 希伯来语       | עברית                |
| hi   | Hindi               | 印地语        | हिन्दी               |
| ho   | Hiri Motu           | 希里莫图语      | Hiri Motu            |
| hr   | Croatian            | 克罗地亚语      | Hrvatski             |
| ht   | Haitian             | 海地克里奥尔语    | Kreyòl ayisyen       |
| hu   | Hungarian           | 匈牙利语       | magyar               |
| hy   | Armenian            | 亚美尼亚语      | Հայերեն              |
| hz   | Herero              | 赫雷罗语       | Otjiherero           |
| ia   | Interlingua         | 因特语        | Interlingua          |
| id   | Indonesian          | 印尼语        | Bahasa Indonesia     |
| ie   | Interlingue         | 西方国际语      | Interlingue          |
| ig   | Igbo                | 伊博语        | Asụsụ Igbo           |
| ii   | Sichuan Yi          | 四川省彝语      | ꆈꌠ꒿ Nuosuhxop        |
| ik   | Inupiaq             | 伊努皮雅克语     | Iñupiaq              |
| io   | Ido                 | 伊多语        | Ido                  |
| is   | Icelandic           | 冰岛语        | Íslenska             |
| it   | Italian             | 意大利语       | Italiano             |
| iu   | Inuktitut           | 因纽特语       | ᐃᓄᒃᑎᑐᑦ               |
| ja   | Japanese            | 日语         | 日本語                  |
| jv   | Javanese            | 爪哇语        | basa Jawa            |
| ka   | Georgian            | 格鲁吉亚语      | ქართული              |
| kg   | Kongo               | 刚果语        | KiKongo              |
| ki   | Kikuyu              | 基库尤语       | Gĩkũyũ               |
| kj   | Kuanyama            | 宽雅玛语       | Kuanyama             |
| kk   | Kazakh              | 哈萨克语       | қазақ тілі           |
| kl   | Kalaallisut         | 格陵兰语       | kalaallisut          |
| km   | Khmer               | 高棉语        | ភាសាខ្មែរ            |
| kn   | Kannada             | 卡纳达语       | ಕನ್ನಡ                |
| ko   | Korean              | 韩语         | 한국어                  |
| kr   | Kanuri              | 卡努里语       | Kanuri               |
| ks   | Kashmiri            | 克什米尔语      | कश्मीरी              |
| ku   | Kurdish             | 库尔德语       | Kurdî                |
| kv   | Komi                | 科米语        | коми кыв             |
| kw   | Cornish             | 康瓦尔语       | Kernewek             |
| ky   | Kyrgyz              | 吉尔吉斯语      | кыргызча             |
| la   | Latin               | 拉丁语        | latine               |
| lb   | Luxembourgish       | 卢森堡语       | Lëtzebuergesch       |
| lg   | Luganda             | 卢干达语       | Luganda              |
| li   | Limburgish          | 林堡语        | Limburgs             |
| ln   | Lingala             | 林加拉语       | Lingála              |
| lo   | Lao                 | 老挝语        | ພາສາລາວ              |
| lt   | Lithuanian          | 立陶宛语       | lietuvių kalba       |
| lu   | Luba-Katanga        | 卢巴语        | Tshiluba             |
| lv   | Latvian             | 拉脱维亚语      | latviešu valoda      |
| mg   | Malagasy            | 马达加斯加语     | Malagasy             |
| mh   | Marshallese         | 马绍尔语       | Kajin M̧ajeļ         |
| mi   | Māori               | 毛利语        | te reo Māori         |
| mk   | Macedonian          | 马其顿语       | македонски јазик     |
| ml   | Malayalam           | 马拉雅拉姆语     | മലയാളം               |
| mn   | Mongolian           | 蒙古语        | монгол               |
| mr   | Marathi             | 马拉地语       | मराठी                |
| ms   | Malay               | 马来语        | bahasa Melayu        |
| mt   | Maltese             | 马耳他语       | Malti                |
| my   | Burmese             | 缅甸语        | မြန်မာ               |
| na   | Nauru               | 瑙鲁语        | Ekakairũ Naoero      |
| nb   | Norwegian           | 挪威语        | norsk bokmål         |
| nd   | North Ndebele       | 北恩德贝勒语     | isiNdebele           |
| ne   | Nepali              | 尼泊尔语       | नेपाली               |
| ng   | Ndonga              | 恩东加语       | Owambo               |
| nl   | Dutch               | 荷兰语        | Nederlands           |
| nn   | Norwegian           | 新挪威语       | nynorsk              |
| no   | Norwegian           | 挪威语        | norsk                |
| nr   | South Ndebele       | 南恩德贝勒语     | isiNdebele           |
| nv   | Navajo              | 纳瓦霍语       | Diné bizaad          |
| ny   | Chichewa            | 尼扬扎语       | chiCheŵa             |
| oc   | Occitan             | 奥克语        | occitan              |
| oj   | Ojibwe              | 奥杰布瓦语      | ᐊᓂᔑᓈᐯᒧᐎᓐ             |
| om   | Oromo               | 奥罗莫语       | Afaan Oromoo         |
| or   | Oriya               | 奥里亚语       | ଓଡ଼ିଆ                |
| os   | Ossetian            | 奥塞梯语       | ирон æвзаг           |
| pa   | Panjabi             | 旁遮普语       | ਪੰਜਾਬੀ               |
| pi   | Pāli                | 巴利语        | पाऴि                 |
| pl   | Polish              | 波兰语        | polski               |
| ps   | Pashto              | 普什图语       | پښتو                 |
| pt   | Portuguese          | 葡萄牙语       | português            |
| qu   | Quechua             | 克丘亚语       | Runa Simi            |
| rm   | Romansh             | 罗曼什语       | rumantsch grischun   |
| rn   | Kirundi             | 基隆迪语       | Ikirundi             |
| ro   | Romanian            | 罗马尼亚语      | română               |
| ru   | Russian             | 俄语         | русский              |
| rw   | Kinyarwanda         | 卢旺达语       | Ikinyarwanda         |
| sa   | Sanskrit            | 梵语         | संस्कृतम्            |
| sc   | Sardinian           | 撒丁语        | sardu                |
| sd   | Sindhi              | 信德语        | सिन्धी               |
| se   | Northern Sami       | 北萨米语       | Davvisámegiella      |
| sg   | Sango               | 桑戈语        | yângâ tî sängö       |
| sh   | Serbo-Croatian      | 塞尔维亚-克罗地亚语 | српскохрватски језик |
| si   | Sinhala             | 僧伽罗语       | සිංහල                |
| sk   | Slovak              | 斯洛伐克语      | slovenčina           |
| sl   | Slovenian           | 斯洛文尼亚语     | slovenščina          |
| sm   | Samoan              | 萨摩亚语       | gagana fa'a Samoa    |
| sn   | Shona               | 修纳语        | chiShona             |
| so   | Somali              | 索马里语       | Soomaaliga           |
| sq   | Albanian            | 阿尔巴尼亚语     | shqip                |
| sr   | Serbian             | 塞尔维亚语      | српски језик         |
| ss   | Swati               | 斯威士语       | SiSwati              |
| st   | Southern Sotho      | 塞索托语       | Sesotho              |
| su   | Sundanese           | 巽他语        | Basa Sunda           |
| sv   | Swedish             | 瑞典语        | svenska              |
| sw   | Swahili             | 斯瓦希里语      | Kiswahili            |
| ta   | Tamil               | 泰米尔语       | தமிழ்                |
| te   | Telugu              | 泰卢固语       | తెలుగు               |
| tg   | Tajik               | 塔吉克语       | тоҷикӣ               |
| th   | Thai                | 泰语         | ไทย                  |
| ti   | Tigrinya            | 提格雷尼亚语     | ትግርኛ                 |
| tk   | Turkmen             | 土库曼语       | Türkmen              |
| tl   | Tagalog             | 他加禄语       | Wikang Tagalog       |
| tn   | Tswana              | 茨瓦纳语       | Setswana             |
| to   | Tonga               | 汤加语        | faka Tonga           |
| tr   | Turkish             | 土耳其语       | Türkçe               |
| ts   | Tsonga              | 宗加语        | Xitsonga             |
| tt   | Tatar               | 塔塔尔语       | татар теле           |
| tw   | Twi                 | 特威语        | Twi                  |
| ty   | Tahitian            | 塔希提语       | Reo Tahiti           |
| ug   | Uyghur              | 维吾尔语       | ئۇيغۇرچە‎            |
| uk   | Ukrainian           | 乌克兰语       | українська           |
| ur   | Urdu                | 乌尔都语       | اردو                 |
| uz   | Uzbek               | 乌兹别克语      | Ўзбек                |
| ve   | Venda               | 文达语        | Tshivenḓa            |
| vi   | Vietnamese          | 越南语        | Tiếng Việt           |
| vo   | Volapük             | 沃拉普克语      | Volapük              |
| wa   | Walloon             | 瓦隆语        | walon                |
| wo   | Wolof               | 沃洛夫语       | Wollof               |
| xh   | Xhosa               | 科萨语        | isiXhosa             |
| yi   | Yiddish             | 依地语        | ייִדיש               |
| yo   | Yoruba              | 约鲁巴语       | Yorùbá               |
| za   | Zhuang              | 壮语         | Saɯ cueŋƅ            |
| zh   | Chinese             | 中文         | 中文                   |
| zu   | Zulu                | 祖鲁语        | isiZulu              |

根据 `ISO3166` 标准, 对于某些语言存在地区变体，使用 `语言代码-国家代码` 的格式：

| 语言代码  | 语言名称                               | 中文名称             | 地区       |
|-------|------------------------------------|------------------|----------|
| zh-CN | Chinese (China)                    | 中文(中国)           | 中国大陆     |
| zh-TW | Chinese (Taiwan)                   | 中文(台湾)           | 中国台湾     |
| zh-HK | Chinese (Hong Kong)                | 中文(香港)           | 中国香港     |
| zh-SG | Chinese (Singapore)                | 中文(新加坡)          | 新加坡      |
| zh-MO | Chinese (Macau)                    | 中文(澳门)           | 中国澳门     |
| en-US | English (US)                       | 英语(美国)           | 美国       |
| en-GB | English (UK)                       | 英语(英国)           | 英国       |
| en-AU | English (Australia)                | 英语(澳大利亚)         | 澳大利亚     |
| en-CA | English (Canada)                   | 英语(加拿大)          | 加拿大      |
| en-NZ | English (New Zealand)              | 英语(新西兰)          | 新西兰      |
| en-IE | English (Ireland)                  | 英语(爱尔兰)          | 爱尔兰      |
| en-ZA | English (South Africa)             | 英语(南非)           | 南非       |
| en-IN | English (India)                    | 英语(印度)           | 印度       |
| en-SG | English (Singapore)                | 英语(新加坡)          | 新加坡      |
| en-MY | English (Malaysia)                 | 英语(马来西亚)         | 马来西亚     |
| en-PH | English (Philippines)              | 英语(菲律宾)          | 菲律宾      |
| en-HK | English (Hong Kong)                | 英语(香港)           | 中国香港     |
| es-MX | Spanish (Mexico)                   | 西班牙语(墨西哥)        | 墨西哥      |
| es-AR | Spanish (Argentina)                | 西班牙语(阿根廷)        | 阿根廷      |
| es-CO | Spanish (Colombia)                 | 西班牙语(哥伦比亚)       | 哥伦比亚     |
| es-CL | Spanish (Chile)                    | 西班牙语(智利)         | 智利       |
| es-PE | Spanish (Peru)                     | 西班牙语(秘鲁)         | 秘鲁       |
| es-VE | Spanish (Venezuela)                | 西班牙语(委内瑞拉)       | 委内瑞拉     |
| es-EC | Spanish (Ecuador)                  | 西班牙语(厄瓜多尔)       | 厄瓜多尔     |
| es-UY | Spanish (Uruguay)                  | 西班牙语(乌拉圭)        | 乌拉圭      |
| es-PY | Spanish (Paraguay)                 | 西班牙语(巴拉圭)        | 巴拉圭      |
| es-BO | Spanish (Bolivia)                  | 西班牙语(玻利维亚)       | 玻利维亚     |
| es-CR | Spanish (Costa Rica)               | 西班牙语(哥斯达黎加)      | 哥斯达黎加    |
| es-PA | Spanish (Panama)                   | 西班牙语(巴拿马)        | 巴拿马      |
| es-DO | Spanish (Dominican Republic)       | 西班牙语(多米尼加)       | 多米尼加     |
| es-GT | Spanish (Guatemala)                | 西班牙语(危地马拉)       | 危地马拉     |
| es-HN | Spanish (Honduras)                 | 西班牙语(洪都拉斯)       | 洪都拉斯     |
| es-NI | Spanish (Nicaragua)                | 西班牙语(尼加拉瓜)       | 尼加拉瓜     |
| es-SV | Spanish (El Salvador)              | 西班牙语(萨尔瓦多)       | 萨尔瓦多     |
| es-CU | Spanish (Cuba)                     | 西班牙语(古巴)         | 古巴       |
| es-PR | Spanish (Puerto Rico)              | 西班牙语(波多黎各)       | 波多黎各     |
| pt-BR | Portuguese (Brazil)                | 葡萄牙语(巴西)         | 巴西       |
| pt-PT | Portuguese (Portugal)              | 葡萄牙语(葡萄牙)        | 葡萄牙      |
| pt-AO | Portuguese (Angola)                | 葡萄牙语(安哥拉)        | 安哥拉      |
| pt-MZ | Portuguese (Mozambique)            | 葡萄牙语(莫桑比克)       | 莫桑比克     |
| pt-GW | Portuguese (Guinea-Bissau)         | 葡萄牙语(几内亚比绍)      | 几内亚比绍    |
| pt-CV | Portuguese (Cape Verde)            | 葡萄牙语(佛得角)        | 佛得角      |
| pt-ST | Portuguese (São Tomé and Príncipe) | 葡萄牙语(圣多美和普林西比)   | 圣多美和普林西比 |
| pt-TL | Portuguese (Timor-Leste)           | 葡萄牙语(东帝汶)        | 东帝汶      |
| fr-CA | French (Canada)                    | 法语(加拿大)          | 加拿大      |
| fr-CH | French (Switzerland)               | 法语(瑞士)           | 瑞士       |
| fr-BE | French (Belgium)                   | 法语(比利时)          | 比利时      |
| fr-LU | French (Luxembourg)                | 法语(卢森堡)          | 卢森堡      |
| fr-MC | French (Monaco)                    | 法语(摩纳哥)          | 摩纳哥      |
| fr-MA | French (Morocco)                   | 法语(摩洛哥)          | 摩洛哥      |
| fr-DZ | French (Algeria)                   | 法语(阿尔及利亚)        | 阿尔及利亚    |
| fr-TN | French (Tunisia)                   | 法语(突尼斯)          | 突尼斯      |
| fr-SN | French (Senegal)                   | 法语(塞内加尔)         | 塞内加尔     |
| fr-CI | French (Ivory Coast)               | 法语(科特迪瓦)         | 科特迪瓦     |
| de-AT | German (Austria)                   | 德语(奥地利)          | 奥地利      |
| de-CH | German (Switzerland)               | 德语(瑞士)           | 瑞士       |
| de-LI | German (Liechtenstein)             | 德语(列支敦士登)        | 列支敦士登    |
| it-CH | Italian (Switzerland)              | 意大利语(瑞士)         | 瑞士       |
| it-SM | Italian (San Marino)               | 意大利语(圣马力诺)       | 圣马力诺     |
| it-VA | Italian (Vatican)                  | 意大利语(梵蒂冈)        | 梵蒂冈      |
| ru-UA | Russian (Ukraine)                  | 俄语(乌克兰)          | 乌克兰      |
| ru-KZ | Russian (Kazakhstan)               | 俄语(哈萨克斯坦)        | 哈萨克斯坦    |
| ru-BY | Russian (Belarus)                  | 俄语(白俄罗斯)         | 白俄罗斯     |
| ru-KG | Russian (Kyrgyzstan)               | 俄语(吉尔吉斯斯坦)       | 吉尔吉斯斯坦   |
| ar-EG | Arabic (Egypt)                     | 阿拉伯语(埃及)         | 埃及       |
| ar-AE | Arabic (UAE)                       | 阿拉伯语(阿联酋)        | 阿联酋      |
| ar-JO | Arabic (Jordan)                    | 阿拉伯语(约旦)         | 约旦       |
| ar-LB | Arabic (Lebanon)                   | 阿拉伯语(黎巴嫩)        | 黎巴嫩      |
| ar-MA | Arabic (Morocco)                   | 阿拉伯语(摩洛哥)        | 摩洛哥      |
| ar-TN | Arabic (Tunisia)                   | 阿拉伯语(突尼斯)        | 突尼斯      |
| ar-DZ | Arabic (Algeria)                   | 阿拉伯语(阿尔及利亚)      | 阿尔及利亚    |
| ar-IQ | Arabic (Iraq)                      | 阿拉伯语(伊拉克)        | 伊拉克      |
| ar-SY | Arabic (Syria)                     | 阿拉伯语(叙利亚)        | 叙利亚      |
| ar-YE | Arabic (Yemen)                     | 阿拉伯语(也门)         | 也门       |
| ar-LY | Arabic (Libya)                     | 阿拉伯语(利比亚)        | 利比亚      |
| ar-SD | Arabic (Sudan)                     | 阿拉伯语(苏丹)         | 苏丹       |
| ar-MR | Arabic (Mauritania)                | 阿拉伯语(毛里塔尼亚)      | 毛里塔尼亚    |
| ar-KW | Arabic (Kuwait)                    | 阿拉伯语(科威特)        | 科威特      |
| ar-QA | Arabic (Qatar)                     | 阿拉伯语(卡塔尔)        | 卡塔尔      |
| ar-BH | Arabic (Bahrain)                   | 阿拉伯语(巴林)         | 巴林       |
| ar-OM | Arabic (Oman)                      | 阿拉伯语(阿曼)         | 阿曼       |
| bn-BD | Bengali (Bangladesh)               | 孟加拉语(孟加拉国)       | 孟加拉国     |
| bn-IN | Bengali (India)                    | 孟加拉语(印度)         | 印度       |
| ur-PK | Urdu (Pakistan)                    | 乌尔都语(巴基斯坦)       | 巴基斯坦     |
| ur-IN | Urdu (India)                       | 乌尔都语(印度)         | 印度       |
| sw-KE | Swahili (Kenya)                    | 斯瓦希里语(肯尼亚)       | 肯尼亚      |
| sw-TZ | Swahili (Tanzania)                 | 斯瓦希里语(坦桑尼亚)      | 坦桑尼亚     |
| sw-UG | Swahili (Uganda)                   | 斯瓦希里语(乌干达)       | 乌干达      |
| se-NO | Northern Sami (Norway)             | 北萨米语(挪威)         | 挪威       |
| se-SE | Northern Sami (Sweden)             | 北萨米语(瑞典)         | 瑞典       |
| se-FI | Northern Sami (Finland)            | 北萨米语(芬兰)         | 芬兰       |
| li-NL | Limburgish (Netherlands)           | 林堡语(荷兰)          | 荷兰       |
| li-BE | Limburgish (Belgium)               | 林堡语(比利时)         | 比利时      |
| yi-US | Yiddish (US)                       | 依地语(美国)          | 美国       |
| yi-IL | Yiddish (Israel)                   | 依地语(以色列)         | 以色列      |
| qu-PE | Quechua (Peru)                     | 克丘亚语(秘鲁)         | 秘鲁       |
| qu-BO | Quechua (Bolivia)                  | 克丘亚语(玻利维亚)       | 玻利维亚     |
| qu-EC | Quechua (Ecuador)                  | 克丘亚语(厄瓜多尔)       | 厄瓜多尔     |
| ay-BO | Aymara (Bolivia)                   | 艾马拉语(玻利维亚)       | 玻利维亚     |
| ay-PE | Aymara (Peru)                      | 艾马拉语(秘鲁)         | 秘鲁       |
| ti-ER | Tigrinya (Eritrea)                 | 提格雷尼亚语(厄立特里亚)    | 厄立特里亚    |
| ti-ET | Tigrinya (Ethiopia)                | 提格雷尼亚语(埃塞俄比亚)    | 埃塞俄比亚    |
| so-DJ | Somali (Djibouti)                  | 索马里语(吉布提)        | 吉布提      |
| so-ET | Somali (Ethiopia)                  | 索马里语(埃塞俄比亚)      | 埃塞俄比亚    |
| so-KE | Somali (Kenya)                     | 索马里语(肯尼亚)        | 肯尼亚      |
| ny-MW | Chichewa (Malawi)                  | 尼扬扎语(马拉维)        | 马拉维      |
| ny-ZW | Chichewa (Zimbabwe)                | 尼扬扎语(津巴布韦)       | 津巴布韦     |
| st-ZA | Southern Sotho (South Africa)      | 塞索托语(南非)         | 南非       |
| st-LS | Southern Sotho (Lesotho)           | 塞索托语(莱索托)        | 莱索托      |
| tn-ZA | Tswana (South Africa)              | 茨瓦纳语(南非)         | 南非       |
| tn-BW | Tswana (Botswana)                  | 茨瓦纳语(博茨瓦纳)       | 博茨瓦纳     |
| ta-IN | Tamil (India)                      | 泰米尔语(印度)         | 印度       |
| ta-LK | Tamil (Sri Lanka)                  | 泰米尔语(斯里兰卡)       | 斯里兰卡     |
| ta-SG | Tamil (Singapore)                  | 泰米尔语(新加坡)        | 新加坡      |
| ta-MY | Tamil (Malaysia)                   | 泰米尔语(马来西亚)       | 马来西亚     |
| pa-IN | Panjabi (India)                    | 旁遮普语(印度)         | 印度       |
| pa-PK | Panjabi (Pakistan)                 | 旁遮普语(巴基斯坦)       | 巴基斯坦     |
| bo-CN | Tibetan (China)                    | 藏语(中国)           | 中国       |
| bo-IN | Tibetan (India)                    | 藏语(印度)           | 印度       |
| sq-AL | Albanian (Albania)                 | 阿尔巴尼亚语(阿尔巴尼亚)    | 阿尔巴尼亚    |
| sq-XK | Albanian (Kosovo)                  | 阿尔巴尼亚语(科索沃)      | 科索沃      |
| sq-MK | Albanian (Macedonia)               | 阿尔巴尼亚语(马其顿)      | 马其顿      |
| sh-BA | Serbo-Croatian (Bosnia)            | 塞尔维亚-克罗地亚语(波黑)   | 波黑       |
| sh-HR | Serbo-Croatian (Croatia)           | 塞尔维亚-克罗地亚语(克罗地亚) | 克罗地亚     |
| sh-ME | Serbo-Croatian (Montenegro)        | 塞尔维亚-克罗地亚语(黑山)   | 黑山       |


---

## 本地化语言贡献指南

### 如何添加新的本地化语言支持

#### 一、复制语言模板文件

 ```bash
 # 从 lang/en.json 复制作为模板
 cp lang/en.json lang/xx-YY.json
 ```
其中 `xx` 是语言代码（如 `zh`, `en`, `ja`），`YY` 是国家/地区代码（如 `CN`, `US`, `CA`）, 如果语言代码和国家/地区代码相同，则省略国家/地区代码(如 `ru-RU` 可省略为 `ru`）。完整语言代码和国家/地区代码请查阅 [语言代码](language-codes.md)

#### 二、更新模板文件内容

编辑新创建的 `lang/xx.json` 文件比如`lang/zh-CN.json`，将所有英文内容翻译为目标语言，以下是一个完整的 `简体中文` 语言文件示例：

 ```json
 {
   "name": "Chinese (China)",
   "author": "https://github.com/your-username",
   "months": "一月|二月|三月|四月|五月|六月|七月|八月|九月|十月|十一月|十二月",
   "short_months": "1月|2月|3月|4月|5月|6月|7月|8月|9月|10月|11月|12月",
   "weeks": "星期日|星期一|星期二|星期三|星期四|星期五|星期六",
   "short_weeks": "周日|周一|周二|周三|周四|周五|周六",
   "seasons": "春季|夏季|秋季|冬季",
   "constellations": "白羊座|金牛座|双子座|巨蟹座|狮子座|处女座|天秤座|天蝎座|射手座|摩羯座|水瓶座|双鱼座",
   "year": "%d 年",
   "month": "%d 个月",
   "week": "%d 周",
   "day": "%d 天",
   "hour": "%d 小时",
   "minute": "%d 分钟",
   "second": "%d 秒",
   "now": "刚刚",
   "ago": "%s前",
   "from_now": "%s后",
   "before": "%s前",
   "after": "%s后"
 }
 ```

##### 字段说明

| 字段               | 说明               | 示例                                 |
|------------------|------------------|------------------------------------|
| `name`           | IOS 语言名称         | "Chinese (China)"                  |
| `author`         | 贡献者的链接           | "https://github.com/your-username" |
| `months`         | 完整月份名称，用 `\|` 分隔 | "一月\|二月\|三月..."                    |
| `short_months`   | 简短月份名称，用 `\|` 分隔 | "1月\|2月\|3月..."                    |
| `weeks`          | 完整星期名称，用 `\|` 分隔 | "星期日\|星期一\|星期二..."                 |
| `short_weeks`    | 简短星期名称，用 `\|` 分隔 | "周日\|周一\|周二..."                    |
| `seasons`        | 季节名称，用 `\|` 分隔   | "春季\|夏季\|秋季\|冬季"                   |
| `constellations` | 星座名称，用 `\|` 分隔   | "白羊座\|金牛座\|双子座..."                 |
| `year`           | 年份格式，支持单复数       | "%d 年"                             |
| `month`          | 月份格式，支持单复数       | "%d 个月"                            |
| `week`           | 周格式，支持单复数        | "%d 周"                             |
| `day`            | 天格式，支持单复数        | "%d 天"                             |
| `hour`           | 小时格式，支持单复数       | "%d 小时"                            |
| `minute`         | 分钟格式，支持单复数       | "%d 分钟"                            |
| `second`         | 秒格式，支持单复数        | "%d 秒"                             |
| `now`            | "现在" 的翻译         | "刚刚"                               |
| `ago`            | "之前" 的翻译         | "%s前"                              |
| `from_now`       | "之后" 的翻译         | "%s后"                              |
| `before`         | "之前" 的翻译         | "%s前"                              |
| `after`          | "之后" 的翻译         | "%s后"                              |

##### 单复数说明

1. **东亚语言（中文、日文、韩文等）**：通常只使用一种格式
   ```json
   "year": "%d 年",
   "month": "%d 个月"
   ```

2. **印欧语言（英文、法文、德文等）**：需要区分单复数
   ```json
   "year": "1 year|%d years",
   "month": "1 month|%d months"
   ```

3. **斯拉夫语言（俄文、乌克兰文等）**：可能有更复杂的复数规则
   ```json
   "year": "1 год|2 года|3 года|4 года|%d лет"
   ```

#### 三、提交 Pull Request

1. **创建分支**
   ```bash
   git checkout -b add-xx-language-support
   ```

2. **提交更改**
   ```bash
   git add lang/xx.json
   git commit -m "add XX language support #39"
   ```

3. **推送并创建 Pull Request**
   ```bash
   git push origin add-xx-language-support
   ```

4. **Pull Request 标题格式**
   ```
   Add XX Language Support #39
   ```

#### 四、测试验证

提交前请确保：

1. **JSON 格式正确**：使用 `JSON` 验证工具检查语法
2. **字段完整**：确保包含所有必需的 `20` 个字段
3. **分隔符正确**：使用 `|` 作为数组分隔符
4. **占位符正确**：使用 `%d` 作为数字占位符，`%s` 作为字符串占位符
5. **保持一致性**：确保翻译风格与现有语言文件保持一致
6. **文化适应性**：考虑目标语言的文化背景和表达习惯

感谢您为 Carbon 项目贡献新的本地化语言支持！


---

## 更新日志
### [v2.6.16](https://github.com/dromara/carbon/compare/v2.6.15...v2.6.16) (2026-01-28)

- 修复 `Format` 方法中`u`、`v`、`x`符号解析错误的 bug
- 将 `actions/checkout`  从 `v5` 升级到 `v6`

### [v2.6.15](https://github.com/dromara/carbon/compare/v2.6.14...v2.6.15) (2025-11-15)

- 移除 `difference.go` 文件的 `DiffAbsInDuration` 方法里使用的从 `go1.19` 才有的 `Abs` 方法
- 修复 `荷兰语` 本地化语言文件(lang/nl.json)里 `weeks` 字段里重复的 `Zondag` 错误
- 增加 `HttpLayout`, `HttpFormat` 常量以及 `ToHttpString` 方法
- 增加对 `蒙古语` 的本地化语言支持(lang/mn.json)
- 增加对 `南非语` 的本地化语言支持(lang/af.json)

### [v2.6.14](https://github.com/dromara/carbon/compare/v2.6.13...v2.6.14) (2025-10-28)

- 优化 `traveler.go` 文件里时间增减系列方法通过复制实例来保证不可变性，以避免修改原始实例
- 增加对 `希腊语` 的本地化语言支持(lang/el.json)
- 增加对 `芬兰语` 的本地化语言支持(lang/fi.json)
- 增加对 `缅甸语` 的本地化语言支持(lang/my.json)

### [v2.6.13](https://github.com/dromara/carbon/compare/v2.6.12...v2.6.13) (2025-10-15)

- 在 `language.go` 的 `SetLocale` 方法中使用 `sync.Once` 确保语言文件只加载一次，使用 `sync.Map` 进行线程安全的缓存
- 在 `helper.go` 的 `format2layout` 方法中为转义字符处理添加边界检查，防止越界访问导致的 `panic`

### [v2.6.12](https://github.com/dromara/carbon/compare/v2.6.11...v2.6.12) (2025-09-16)

- 将 `golang` 环境依赖从 `1.21` 降低到 `1.18`
- 将 `testify` 测试框架从 `v1.10.0` 升级到 `v1.11.1`
- 在 `type_carbon.go` 文件 `UnmarshalJSON` 方法中设置 `isEmpty` 标志以表示空值
- 使用 `sync.Map` 实现高性能并发缓存
- 修复潜在的竞态条件和空指针解引用问题，提高并发安全性

### [v2.6.11](https://github.com/dromara/carbon/compare/v2.6.10...v2.6.11) (2025-07-18)

- 将 `Sleep` 由结构体方法更改成全局方法
- 重构 `波斯历` 并添加基准测试文件
- 重构 `中国农历` 并添加基准测试文件
- 重构 `儒略日/简化儒略日` 并添加基准测试文件
- 新增 `希伯来历` 支持并添加单元测试和基准测试文件
- 新增整体性能测试报告文件

### [v2.6.10](https://github.com/dromara/carbon/compare/v2.6.9...v2.6.10) (2025-07-07)

- 将`日语`翻译文件从 `jp.json` 改成 `ja.json`，说明文档从 `README.jp.md` 更名为 `README.ja.md`，以符合 [ISO639-1](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes) 标准
- 移除已弃用的 `ParseWithLayouts` 方法，用 `ParseByLayouts` 方法替代
- 移除已弃用的  `ParseWithFormats` 方法，用 `ParseByFormats` 方法替代
- 移除已弃用的 `CleanTestNow` 方法，用 `ClearTestNow` 方法替代
- 移除 `ParseByLayout` 和 `ParseByFormat` 方法对`时间戳`字符串的解析支持，解析`时间戳`请使用 `CreateFromTimestamp`, `CreateFromTimestampMilli`, `CreateFromTimestampMicro`, `CreateFromTimestampNano` 方法
- 优化 `helper.go` 里 `getAbsValue` 方法，用`位操作`替换条件判断
- 优化 `frozen.go` 文件里时间冻结相关方法，用`原子操作`减少锁竞争，优化内存分配
- 优化基准测试文件，覆盖`串行测试`、`并行测试`和`并发测试`
- 新增韩语文档 `README.ko.md`
- 新增 `Sleep` 方法及相关`单元测试`、`基准测试`和`示例文件`
- 新增数字常量，如 `MaxYear`, `MinYear`, `MaxMonth`, `MinMonth`, `MaxDay`, `MinDay` 等，并使用这些常量替换硬编码

### [v2.6.9](https://github.com/dromara/carbon/compare/v2.6.7...v2.6.8) (2025-06-28)

- 移除对 `gorm` 的 `GormDataType` 接口的实现

### [v2.6.8](https://github.com/dromara/carbon/compare/v2.6.7...v2.6.8) (2025-06-12)

- 解析时如果布局模板或格式模板为空时，返回错误
- 在 `tests` 中将 `gorm.io/gorm` 从 `1.21.1` 升级到 `1.30.0`
- 在 `tests` 中将 `gorm.io/driver/mysql` 从 `1.5.7` 升级到 `1.6.0`
- 在 `tests` 中将 `gorm.io/driver/postgres` 从 `1.5.7` 升级到 `1.6.0`
- 在 `tests` 中将 `gorm.io/driver/sqlite` 从 `1.5.7` 升级到 `1.6.0`
- 在 `type_builtin.go` 中将 `DateTimeType` 重命名为`dateTimeType`，将 `DateTimeXXXType` 重命名为 `dateTimeXXXType`
- 在 `type_builtin.go` 中将 `DateType` 重命名为`dateType`，将 `DateXXXType` 重命名为 `dateXXXType`
- 在 `type_builtin.go` 中将 `TimeType` 重命名为`timeType`，将 `TimeXXXType` 重命名为 `timeXXXType`
- 简化 READEME 文件，将详细使用说明和示例用法迁移到 [官方网站](https://carbon.go-pkg.com)
- 添加 [HelloGitHub](https://hellogithub.com/repository/dromara/carbon) 徽章链接

### [v2.6.7](https://github.com/dromara/carbon/compare/v2.6.6...v2.6.7) (2025-05-26)

-  `String` 方法去掉对空值的检查
- 将 `type_interface.go` 更名为 `interfaces.go`
- 将`Closest`/`Farthest` 方法第 2 个参数改成可选参数
- 新增 `ZeroValue`/`EpochValue` 方法
- 新增 `DataTyper` 接口和 `DataType`方法并让内置类型实现 `DataTyper` 接口

### [v2.6.6](https://github.com/dromara/carbon/compare/v2.6.5...v2.6.6) (2025-05-19)

- 修复在 `window` 平台无法找到语言文件的 bug
- 修复在创建新的 `Carbon` 实例时丢失`layout`、`weekStartsAt`、`weekendDays` 和 `lang 值的错误
- 修复 `StartOfWeek` 和 `EndOfWeek`方法意外更改原始 `Carbon`实例的错误 
- 新增对 `xorm` 的 `curd` 集成测试，目前已覆盖 `MySQL`/`Postgres`/`SQLite`
- 在  `ci` 中新增 `window` 系统的单元测试

### [v2.6.5](https://github.com/dromara/carbon/compare/v2.6.4...v2.6.5) (2025-05-14)

- 将 `Go` 最低版本要求从 `1.18` 提升到 `1.21`
- `Carbon` 结构体的 `SetLanguage` 方法增加对非法 `Language` 结构体的判断
- `Carbon` 结构体的`Parse` 方法增加对 `Postgres`/`SQLite` 时间格式字符串的解析支持
- `Carbon` 结构体的`Parse`/`ParseByLayout`/`ParseByFormat`方法解析 `空字符串` 时返回值从 `nil` 更改成空 `carbon`
- `Carbon` 结构体新增 `IsEmpty` 方法用于判断是否为空 `carbon`
- `Carbon` 结构体新增 `ClearTestNow` 方法替代 `CleanTestNow`, `CleanTestNow` 方法未来将移除
- `Carbon` 结构体新增 `ParseByLayouts` 方法替代 `ParseWithLayouts`, `ParseWithLayouts` 方法未来将移除
- `Carbon` 结构体新增 `ParseByFormats` 方法替代 `ParseWithLayouts`, `ParseWithFormats` 方法未来将移除
- `Carbon` 结构体移除 `GormDataType `方法, 并将 `Value`/`MarshalJSON `方法从`指针`接收者改成`值`接收者
- `LayoutType[T]` 结构体移除 `GormDataType `方法, 并将 `Value`/`MarshalJSON `方法从`指针`接收者改成`值`接收者
- `FormatType[T]` 结构体移除 `GormDataType `方法, 并将 `Value`/`MarshalJSON `方法从`指针`接收者改成`值`接收者
- `TimestampType[T]` 结构体移除 `GormDataType `方法, 并将 `Value`/`MarshalJSON `方法从`指针`接收者改成`值`接收者
- `Language` 结构体的 `SetResources` 方法增加对非法资源的判断
- 新增对 `gorm` 的 `curd` 集成测试，目前已覆盖 `MySQL`/`Postgres`/`SQLite`
- 使用 `github.com/stretchr/testify/assert` 替换 `github.com/stretchr/testify/suite` 进行单元测试

### [v2.6.4](https://github.com/dromara/carbon/compare/v2.6.3...v2.6.4) (2025-04-28)

-  修复数据库字段类型为 `nil` 时抛出异常的bug
- 将 `database_types.go` 拆分成 `type_carbon.go`, `type_layout.go`, `type_format.go`, `type_timestamp.go`
- 将 `LayoutFactory`  接口重命名为 `LayoutTyper` 和  `SetLayout` 方法重命名为 `Layout`
- 将 `FormatFactory`  接口重命名为 `FormatTyper` 和 `SetFormat` 方法重命名为 `Format`
- 将 `TimestampFactory`  接口重命名为 `TimestampTyper` 和 `SetPrecision` 方法重命名为 `Precision`
- 性能测试文件增加 `b.ResetTimer()`
- `Language` 结构体新增 `Copy` 方法
- 新增 `carbon.Timestamp` 类型别名和 `carbon.NewTimestamp` 方法
- 新增 `carbon.TimestampMilli` 类型别名和 `carbon.NewTimestampMilli` 方法
- 新增 `carbon.TimestampMicro` 类型别名和 `carbon.NewTimestampMicro` 方法
- 新增 `carbon.TimestampNano` 类型别名和 `carbon.NewTimestampNano` 方法
- 新增 `carbon.DateTime` 类型别名和 `carbon.NewDateTime` 方法
- 新增 `carbon.DateTimeMicro` 类型别名和 `carbon.NewDateTimeMicro` 方法
- 新增 `carbon.DateTimeMilli` 类型别名和 `carbon.NewDateTimeMilli` 方法
- 新增 `carbon.DateTimeNano` 类型别名和 `carbon.NewDateTimeNano` 方法
- 新增 `carbon.Date` 类型别名和 `carbon.NewDate` 方法
- 新增 `carbon.DateMilli` 类型别名和 `carbon.NewDateMilli` 方法
- 新增 `carbon.DateMicro` 类型别名和 `carbon.NewDateMicro` 方法
- 新增 `carbon.DateNano` 类型别名和 `carbon.NewDateNano` 方法
- 新增 `carbon.Time` 类型别名和 `carbon.NewTime` 方法
- 新增 `carbon.TimeMilli` 类型别名和 `carbon.NewTimeMilli` 方法
- 新增 `carbon.TimeMicro` 类型别名和 `carbon.NewTimeMicro` 方法
- 新增 `carbon.TimeNano` 类型别名和 `carbon.NewTimeNano` 方法

### [v2.6.3](https://github.com/dromara/carbon/compare/v2.6.2...v2.6.3) (2025-04-21)

-  修复 `IsWeekend`, `IsWeekday` 方法不同国家返回结果一致的 bug
-  修复 `StdTime` 方法空指针引起的异常 #294
-  将错误方法由 `私有` 方法改成 `公开` 方法
-  将一周默认开始日期从 `周日` 改成 `周一`
-  将 `MinValue` 方法的年份从 `-9998` 更改为 `1`
-  将 `weeksPerLongYear` 常量更名为 `WeeksPerLongYear`
-  新增性能测试文件 `xxx_bench_test.go`
-  新增 `IsEpoch` 方法用于判断是否是 UNIX 纪元时间(1970-01-01 00:00:00 +0000 UTC)
-  新增 `WeekEndsAt` 方法用于获取一周的结束日期
-  新增 `SetWeekendDays` 方法用于设置一周周末日期
-  新增 `DefaultWeekStartsAt` 全局变量用于存储默认一周休息日

### [v2.6.2](https://github.com/dromara/carbon/compare/v2.6.1...v2.6.2) (2025-04-08)

-  `CreateFromLunar`, `CreateFromPersian` 方法去掉 hour, minute, second 参数
- 更改部分格式符号定义，涉及到的符号有 `U`, `V`, `X`,`S`,`T` `Z`,`u`,`v`,`x`,`z`
- 修复农历中 `IsLeapMonth` 判断错误的 bug
- 修复 `AtomFormat` 和 `AtomLayout` 格式返回值不一致的 bug
- 修复 `RFC3339Format` 和 `RFC3339Layout` 格式返回值不一致的 bug
- 设置全局默认时区时不再同步更新 `time.Local`
- 新增格式符号`o` 来获取时区偏移量
- 新增 `TimestampLayout`、`TimestampMilliLayout`、`TimestampMicroLayout` 和 `TimestampNanoLayout` 常量
- 新增 `TimestampFormat`、`TimestampMilliFormat`、`TimestampMicroFormat` 和 `TimestampNanoFormat` 常量
- 新增 `DateTimeMilli`、`DateTimeMicro`、`DateTimeNano` 字段类型
- 新增 `DateMilli`、`DateMicro`、`DateNano` 字段类型
- 新增 `TimeMilli`、`TimeMicro`、`TimeNano` 字段类型
- 修复 `IsDST` 方法丢失时区的 bug
- 修复 `StartOfXXX`、`EndOfXXX` 部分方法丢失时区的 bug
- 修复其他日历转化为公历时缺失时区的 bug
- 设置默认时区时不再同步更新  `time.Local`
- 新增 `MaxDuration`、`MinDuration` 方法

### [v2.6.1](https://github.com/dromara/carbon/compare/v2.6.0...v2.6.1) (2025-03-27)

-  新增 `ParseWithLayouts`  和 `ParseWithFormats` 方法
-  将 `formatFactory` 接口更名为 `FormatFactory`, `formatFactory` 接口更名为 `FormatFactory`, `formatFactory` 接口更名为 `FormatFactory`, 并添加类型约束
-  将 `LayoutType` ,`FormatType`, `TimestampType` 结构体`GormDataType` 方法的返回值更改为 `time`
-  将 `DateTime`、`Date`、`Time` 类型从 `struct` 更改为 `string`
-  将`Timestamp`、`TimestampMilli`、`TimestampMicro`,`TimestampNano` 类型从 `struct` 更改为 `int64`
-  将内置数据库字段类型移动到新文件 `types.go`
-  修复 `gorm` 更新数据时 `updated_at` 字段自动更新无效的 bug

### [v2.6.0](https://github.com/dromara/carbon/compare/v2.5.4...v2.6.0) (2025-03-25)

- `golang` 最低版本依赖升级到 `1.18`
- `carbon`, `julian`, `lunar`, `persian` 从值传递改成指针传递
-  新增 `ZoneName` 方法获取时区名称
-  新增 `HasError` 方法判断是否有错误
-  新增 `IsNil` 方法判断是否是 `nil`
-  新增 `Copy` 方法对 `carbon` 进行深度复制
-  新增 `WeekStartsAt` 方法获取周起始日期
-  新增示例文件 `xxx_example.go`
-  新增`constant.go` 文件，将常量从 `carbon.go` 文件迁移到此文件
-  默认全局时区从 `Local` 更改为 `UTC`
-  `Offset` 方法更名为 `ZoneOffset`
-  `IsSetTestNow` 方法更名为 `IsTestNow`
-  `UnSetTestNow ` 方法更名为 `CleanTestNow`
-  移除 `Location` 方法，由 `Timezone` 方法替代
-  更改 `IsValid` 和 `IsInvalid` 方法判断逻辑，`zero time` 不再视为无效时间
-  设置全局默认时区时同步更新 `time.Local`
-  重构 `database.go`，移除 `carbon.DateTime`、`carbon. DateTimeMilli `、 `carbon.DateTimeMicro`、`carbon.DateTimeNano`、 `carbon. Date`、`carbon.DateMilli`、 `carbon.DateMicro`、 `carbon.DateNano`、 `carbon.Time`、 `carbon.TimeMilli`、 `carbon.TimeMicro`、  `carbon.TimeNano`、`carbon.Timestamp` 、`carbon.TimestampMilli ` 、`carbon.TimestampMicro`、`carbon.TimestampNano` 字段类型, 使用泛型字段替代以实现 `MarshalJSON/UnmarshalJSON` 时自定义输出格式

有关更早版本的更新日志，请参阅 <a href="https://github.com/dromara/carbon/releases" target="_blank" rel="noreferrer">releases</a>

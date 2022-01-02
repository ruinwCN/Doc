### 0.builder.go

注意，如果使用了strings.Builder ，很多操作之前都会判断是否可以copy，被赋值之后就无法进行copy。

```
func (b *Builder) copyCheck() {
	if b.addr == nil {
		// This hack works around a failing of Go's escape analysis
		// that was causing b to escape and be heap allocated.
		// See issue 23382.
		// TODO: once issue 7921 is fixed, this should be reverted to
		// just "b.addr = b".
		b.addr = (*Builder)(noescape(unsafe.Pointer(b)))
	} else if b.addr != b {
		panic("strings: illegal use of non-zero Builder copied by value")
	}
}
```

### 	1. replace.go

字符串替换

主要由提供字符串替换功能

```
// Replace returns a copy of the string s with the first n
// non-overlapping instances of old replaced by new.
// If old is empty, it matches at the beginning of the string
// and after each UTF-8 sequence, yielding up to k+1 replacements
// for a k-rune string.
// If n < 0, there is no limit on the number of replacements.
func Replace(s, old, new string, n int) string {}

```

```
// ReplaceAll returns a copy of the string s with all
// non-overlapping instances of old replaced by new.
// If old is empty, it matches at the beginning of the string
// and after each UTF-8 sequence, yielding up to k+1 replacements
// for a k-rune string.
func ReplaceAll(s, old, new string) string {
   return Replace(s, old, new, -1)
}
```

通过内置的trie-tree快速查找替换。



### 2. strings.go

使用了Buffer升级。存储空间改动维护，业务类代码

#### Index(s, substr string) int

会根据substring的长度选择不一样的算法。

```bytealg.MaxLen```和cpu有有关系 默认值在/bytealg/cpu/

失败次数过多，就会使用RabinKarp算法

```
j := bytealg.IndexRabinKarp(s[i:], substr)
```

#### 
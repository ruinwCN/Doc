### 0.buffer.go

模块error定义，直接定义

```
// ErrTooLarge is passed to panic if memory cannot be allocated to store data in a buffer.
var ErrTooLarge = errors.New("bytes.Buffer: too large")
var errNegativeRead = errors.New("bytes.Buffer: reader returned negative count from Read")
```

[]bytes add string 

```
copy(b.buf[m:], s)
```

```
func NewBufferString(s string) *Buffer {
	return &Buffer{buf: []byte(s)}
}
```

对Rune 特殊处理，通过 UTF-8 编码之后再写入， 和byte处理不一样的地方。

```
// WriteRune appends the UTF-8 encoding of Unicode code point r to the
// buffer, returning its length and an error, which is always nil but is
// included to match bufio.Writer's WriteRune. The buffer is grown as needed;
// if it becomes too large, WriteRune will panic with ErrTooLarge.
func (b *Buffer) WriteRune(r rune) (n int, err error) {
	// Compare as uint32 to correctly handle negative runes.
	if uint32(r) < utf8.RuneSelf {
		b.WriteByte(byte(r))
		return 1, nil
	}
	b.lastRead = opInvalid
	m, ok := b.tryGrowByReslice(utf8.UTFMax)
	if !ok {
		m = b.grow(utf8.UTFMax)
	}
	n = utf8.EncodeRune(b.buf[m:m+utf8.UTFMax], r)
	b.buf = b.buf[:m+n]
	return n, nil
}
```


read option
```
// Don't use iota for these, as the values need to correspond with the
// names and comments, which is easier to see when being explicit.
const (
	opRead      readOp = -1 // Any other read operation.
	opInvalid   readOp = 0  // Non-read operation.
	opReadRune1 readOp = 1  // Read rune of size 1.
	opReadRune2 readOp = 2  // Read rune of size 2.
	opReadRune3 readOp = 3  // Read rune of size 3.
	opReadRune4 readOp = 4  // Read rune of size 4.
)
```
对应
```
	rune1Max = 1<<7 - 1
	rune2Max = 1<<11 - 1
	rune3Max = 1<<16 - 1
```
Unread的时候减去相对应的长度

### 1.reader.go

实现了

io.Reader, io.ReaderAt, io.WriterTo, io.Seeker,io.ByteScanner, and io.RuneScanner interfaces

在操作rune时候一样需要注意Rune需要decode

```
func DecodeRune(p []byte) (r rune, size int)
```

### 2. bytes.go

Index 和 strings是一样的。 根据长度选择具体算法。



tips:  simple export func

```
// Export func for testing
var IndexBytePortable = indexBytePortable
```

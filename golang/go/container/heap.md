## Heap

### 0. Code

外置实现接口

```
	Len() int
	// Less reports whether the element with
	// index i should sort before the element with index j.
	Less(i, j int) bool
	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
	
	// 
	Push(x interface{}) // add x as element Len()
	Pop() interface{}   // remove and return element Len() - 1.
```

核心

```
func down(h Interface, i0, n int) bool {
	i := i0
	for {
		j1 := 2*i + 1
		if j1 >= n || j1 < 0 { // j1 < 0 after int overflow
			break
		}
		j := j1 // left child
		if j2 := j1 + 1; j2 < n && h.Less(j2, j1) {
			j = j2 // = 2*i + 2  // right child
		}
		if !h.Less(j, i) {
			break
		}
		h.Swap(i, j)
		i = j
	}
	return i > i0
}
```

完全二叉树，从最小非叶子节点开始比较O(logn)时间复杂度。只保证root最小



### 1. interface{} 模式示例

```
package main

import "fmt"

type sound interface {
	Sound() string
	Doing(string) string
}

type Action interface {
	sound
	Touch()
}

func TouchPlay(action Action, play string) string {
	action.Touch()
	return action.Doing(play)
}

type Dog struct {
}

func (a *Dog) Touch() {
}

func (a *Dog) Sound() string {
	return "wo"
}

func (a *Dog) Doing(s string) string {
	return fmt.Sprintf("%s and  pay %s ", a.Sound(), s)
}

type Cat struct {
}

func (c *Cat) Touch() {
	fmt.Println("cat touch")
}

func (c *Cat) Sound() string {
	return "mio"
}

func (a *Cat) Doing(s string) string {
	return fmt.Sprintf("%s  and  pay %s ", a.Sound(), s)
}

func main() {
	a := Dog{}
	c := Cat{}
	fmt.Println(TouchPlay(&a, "ball"))
	fmt.Println(TouchPlay(&c, "123"))
}

```


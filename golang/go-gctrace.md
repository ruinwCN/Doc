[toc]

## gctrace

gctrace主要是用于跟踪GC的不同阶段的耗时与GC前后的内存对比，gctrace可以简单的查看内存使用状态以及判断是否有内存泄漏，更加详细的信息可以通过pprof获取。



在 [runtime pkg](https://golang.org/pkg/runtime/) 中可以看到gc的一些字段，通过信息我们可以看到每个gc开始的时间，使用的堆内存等信息。

```
where the fields are as follows:
	gc #        the GC number, incremented at each GC
	@#s         time in seconds since program start
	#%          percentage of time spent in GC since program start
	#+...+#     wall-clock/CPU times for the phases of the GC
	#->#-># MB  heap size at GC start, at GC end, and live heap
	# MB goal   goal heap size
	# P         number of processors used
```

## 使用说明

首先需要使用内存追踪模式`GODEBUG=gctrace=1 ` 启动程序之后，我们就可以在控制台查看内存和调度器的trace。

自动回收的时候会打印信息。

```
# command-line-arguments
gc 1 @0.004s 2%: 0.010+1.5+0.007 ms clock, 0.13+0.13/1.6/1.8+0.084 ms cpu, 4->5->4 MB, 5 MB goal, 12 P
# command-line-arguments
gc 1 @0.001s 10%: 0.011+1.5+0.017 ms clock, 0.13+0.21/3.6/0.89+0.21 ms cpu, 4->6->5 MB, 5 MB goal, 12 P
gc 2 @0.008s 5%: 0.007+1.0+0.003 ms clock, 0.092+0/2.0/1.7+0.046 ms cpu, 9->9->6 MB, 10 MB goal, 12 P
gc 3 @0.019s 3%: 0.017+2.4+0.018 ms clock, 0.20+0.091/2.7/0.58+0.22 ms cpu, 12->12->8 MB, 13 MB goal, 12 P
gc 4 @0.048s 2%: 0.020+2.8+0.036 ms clock, 0.24+0.053/2.8/0.12+0.44 ms cpu, 15->15->9 MB, 16 MB goal, 12 P
gc 1 @0.002s 87%: 22+0.47+0.11 ms clock, 269+0.25/0.27/0+1.3 ms cpu, 459->459->459 MB, 460 MB goal, 12 P
```

文档中列出了可以选择的参数，比如gcpacertrace, gcstoptheworld等

还可以通过设置`schedtrace=X` ，运行之后每Xms发出一个调度器摘要信息,同时加上`scheddetail=1` 可以看到更多信息：

```
  P9: status=0 schedtick=0 syscalltick=0 m=-1 runqsize=0 gfreecnt=0 timerslen=0
  P10: status=0 schedtick=0 syscalltick=0 m=-1 runqsize=0 gfreecnt=0 timerslen=0
  P11: status=0 schedtick=0 syscalltick=0 m=-1 runqsize=0 gfreecnt=0 timerslen=0
  M5: p=2 curg=-1 mallocing=0 throwing=0 preemptoff= locks=1 dying=0 spinning=true blocked=false lockedg=-1
  M4: p=0 curg=-1 mallocing=0 throwing=0 preemptoff= locks=1 dying=0 spinning=true blocked=false lockedg=-1
  M3: p=1 curg=-1 mallocing=0 throwing=0 preemptoff= locks=1 dying=0 spinning=false blocked=false lockedg=-1
  M2: p=-1 curg=-1 mallocing=0 throwing=0 preemptoff= locks=2 dying=0 spinning=false blocked=false lockedg=-1
  M1: p=-1 curg=17 mallocing=0 throwing=0 preemptoff= locks=0 dying=0 spinning=false blocked=false lockedg=17
  M0: p=-1 curg=-1 mallocing=0 throwing=0 preemptoff= locks=0 dying=0 spinning=false blocked=true lockedg=1
  G1: status=1(chan receive) m=-1 lockedm=0
  G17: status=6() m=1 lockedm=1
  G2: status=4(force gc (idle)) m=-1 lockedm=-1
```

 比如GMP的信息，G的运行状态，所属的M和P的运行状态等等。

Code:

> https://github.com/ruinwCN/example/blob/master/go/go-gctrace/example-go-gctrance.go






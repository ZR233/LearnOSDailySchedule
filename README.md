# LearnOSDailySchedule

这是我关于rCore的学习文档。

## Week 1-2 (一阶段)

### 计划

由于已有部分Rust编程基础，这一阶段主要目的是巩固已有知识，学习高级用法。

   1. 阅读Rust程序设计
   2. 阅读Rust编程之道
   3. 利用rustling对已有知识查缺补漏

### 成果

   1. 经过对资料的系统阅读，发现之前的理解有部分错误，提高了对Rust这门语言的认识。
   2. 学习了Rust语言对于并发的安全控制，加深了对于并发编程的理解。
   3. 通过rustling学习到了iterator的一些语法糖，提高了编程效率和代码美观性。

## 第二阶段

## Day 1

计划：

   1. 下班后听课。
   2. 看一下第一章。

成果：

   1. 看了第一章，时间不早，睡觉了。  
  
## Day 2

计划：

   1. 配置实验环境。

成果：

   1. 实验文档推荐Qemu 7.0.0 环境，但本着学习环境用新不用旧的原则，尝试搭建8.0环境，果然跑不起来，输出卡死。在学习群中发现有解决方案，更新rustsbi-qemu.bin解决。

## Day 3

计划：

   1. 下班后听课。
   2. 学习第二章第三章。

成果：

   1. 完成任务

## Day 4

计划：

   1. 做ch3题目。

过程：

阅读了代码，多任务切换的实现，主要依靠系统调用：

```rust
// user/src/syscall.rs

pub fn sys_yield() -> isize {
    syscall(SYSCALL_YIELD, [0, 0, 0])
}

// user/src/lib.rs
pub fn yield_() -> isize { sys_yield() }

```

以及逻辑部分 `TaskManager TaskControlBlock` 。`TaskManager` 负责任务的创建销毁以及调度，`TaskControlBlock` 储存了当前任务信息。因此要完成题目，获取`TaskInfo`，可以从`TaskControlBlock`入手，在其中添加字段`pub syscall_times: [u32; MAX_SYSCALL_NUM],`
用以保存系统调用计数，添加 `start_at: usize` 字段保存任务开始时间。
增加`get_task_info`函数，获取当前任务，并将任务状态、调用次数、持续时间输出。
在 `syscall`函数中添加计数逻辑，为所有系统调用增加调用统计。

成果：

   1. 成功实现题目目标，并理解了rCore的任务调度系统。

## Day 5

计划：

1. 下班听课。
2. 学习第四章内容。

成果：

1. 课听完了。
2. 学的云里雾里，睡了。

## Day 6

计划：

1. 搞定ch4。

过程：

在`page_tables.rs`中有`translated_byte_buffer`函数，通过阅读其逻辑，理解了从虚拟地址指针到物理地址变换的方式。将`*const T`类型转换为`usize`，再将其转换为`VirtAddr`，再将其转换为`VirtPageNum`，再通过`pange_table`找到物理页地址。利用相似原理，实现`get_time`和`get_task_info`函数。

为实现mmap，需要为每个task建立一个页表映射，好在task中的memoryset已存在page_tables字段，而要保持创建的frame不被析构，则需要再添加一个字段以保存BTreeMap，在此思路之下，完成了mmap系统调用。

## Day7

计划：

1. 搞定ch5。

过程：

这一章节感觉较前一章节简单一些，根据实验文档中user_shell的实现，利用现有的fork与exec实现spawn函数。
在`TaskControlBlockInner`中添加stride调度所需要的字段，修改`TaskManager`中获取下一个任务的逻辑，以满足算法要求。

成果：

1. 理解了进程的创建原理，认识了stride调度的实现方式。

## Day8

1. 下班听课。
2. 学习第六章内容。

成果：

1. 课听完了。
2. 阅读了源码。

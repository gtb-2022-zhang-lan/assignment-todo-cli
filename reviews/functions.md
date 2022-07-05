# 功能实现 review 结果

## Summary

### Level-1

1. 🔴 Scenario: 初始化后，任务列表默认为空

1. 🔴 Scenario: 添加 3 个任务并可以正常显示

### Level-2

1. 🔴 Scenario: 可以添加 10 个及以上的任务并且列表格式正确

1. 🔴 Scenario: mark TBD 里的第一个任务

1. 🔴 Scenario: mark TBD 里的多个任务后可以再继续正常 add 新 task

1. 🔴 Scenario: mark TBD 里的所有任务

### Level-3

1. 🔴 Scenario: 有 tasks 时再运行 init 不会破坏已有数据

1. 🟢 Scenario: 执行任何 child command 之前必须先执行 init

1. 🔴 Scenario: 可以添加标题包含空格及其它符号的 task

1. 🔴 Scenario: mark 任务时没有提供任何 ID

1. 🔴 Scenario: 同时 mark 多个已 completed、不存在及已删除的任务

## Details

### Level-1

#### 🔴 Scenario: 初始化后，任务列表默认为空

Steps:

```bash
rm -rf ~/.todo
todo init
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:44 2022$
***************$
*** 1,5 ****$
! Initialized successfully.$
! # To be done$
! Empty$
! # Completed$
! Empty$
--- 1,2 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'list' command.$
```

#### 🔴 Scenario: 添加 3 个任务并可以正常显示

Steps:

```bash
rm -rf ~/.todo
todo init
todo add Task 001
todo add Task 002
todo add Task 003
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:44 2022$
***************$
*** 1,7 ****$
! Initialized successfully.$
! # To be done$
! 1 Task 001$
! 2 Task 002$
! 3 Task 003$
! # Completed$
! Empty$
--- 1,5 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
```

### Level-2

#### 🔴 Scenario: 可以添加 10 个及以上的任务并且列表格式正确

Steps:

```bash
rm -rf ~/.todo
todo init
for id in $(seq -w 12)
do
    todo add Task "${id}"
done
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,16 ****$
! Initialized successfully.$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! 3 Task 03$
! 4 Task 04$
! 5 Task 05$
! 6 Task 06$
! 7 Task 07$
! 8 Task 08$
! 9 Task 09$
! 10 Task 10$
! 11 Task 11$
! 12 Task 12$
! # Completed$
! Empty$
--- 1,14 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
```

#### 🔴 Scenario: mark TBD 里的第一个任务

Steps:

```bash
rm -rf ~/.todo
todo init
todo add Task 01
todo add Task 02
todo add Task 03
todo list
todo mark 1
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,12 ****$
! Initialized successfully.$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! 3 Task 03$
! # Completed$
! Empty$
! # To be done$
! 2 Task 02$
! 3 Task 03$
! # Completed$
! 1 Task 01$
--- 1,7 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
```

#### 🔴 Scenario: mark TBD 里的多个任务后可以再继续正常 add 新 task

Steps:

```bash
rm -rf ~/.todo
todo init
todo add Task 01
todo add Task 02
todo add Task 03
todo add Task 04
todo add Task 05
todo list
todo mark 1 3 5
todo list
todo add Task 06
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,24 ****$
! Initialized successfully.$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! 3 Task 03$
! 4 Task 04$
! 5 Task 05$
! # Completed$
! Empty$
! # To be done$
! 2 Task 02$
! 4 Task 04$
! # Completed$
! 1 Task 01$
! 3 Task 03$
! 5 Task 05$
! # To be done$
! 2 Task 02$
! 4 Task 04$
! 6 Task 06$
! # Completed$
! 1 Task 01$
! 3 Task 03$
! 5 Task 05$
--- 1,11 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
```

#### 🔴 Scenario: mark TBD 里的所有任务

Steps:

```bash
rm -rf ~/.todo
todo init
todo add Task 01
todo add Task 02
todo add Task 03
todo list
todo mark 1 2 3
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,13 ****$
! Initialized successfully.$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! 3 Task 03$
! # Completed$
! Empty$
! # To be done$
! Empty$
! # Completed$
! 1 Task 01$
! 2 Task 02$
! 3 Task 03$
--- 1,7 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
```

### Level-3

#### 🔴 Scenario: 有 tasks 时再运行 init 不会破坏已有数据

Steps:

```bash
rm -rf ~/.todo
todo init
seq 1 4 | xargs -I_ todo add Task 0_
todo mark 4 3
todo list
todo init
todo list
todo add Task 05
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,21 ****$
! Initialized successfully.$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! # Completed$
! 3 Task 03$
! 4 Task 04$
! Initialized successfully.$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! # Completed$
! 3 Task 03$
! 4 Task 04$
! # To be done$
! 1 Task 01$
! 2 Task 02$
! 5 Task 05$
! # Completed$
! 3 Task 03$
! 4 Task 04$
--- 1,11 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
```

#### 🟢 Scenario: 执行任何 child command 之前必须先执行 init

Steps:

```bash
rm -rf ~/.todo
todo list 2>&1
echo $?
todo add 2>&1
echo $?
todo mark 2>&1
echo $?
```

#### 🔴 Scenario: 可以添加标题包含空格及其它符号的 task

Steps:

```bash
rm -rf ~/.todo
todo init
todo list
todo add 
todo add
todo add foo bar
todo add "foo bar"
todo add "foo   bar"
todo add "   foo   bar   "
todo add 9
todo add 10
todo add 书山有路勤为径！
todo add '#@!$%^&*()|<>?'
todo list
todo mark 7 8
todo list
todo mark {1..6} 9 10
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,43 ****$
! Initialized successfully.$
! # To be done$
! Empty$
! # Completed$
! Empty$
! # To be done$
! 1 $
! 2 $
! 3 foo bar$
! 4 foo bar$
! 5 foo   bar$
! 6    foo   bar   $
! 7 9$
! 8 10$
! 9 M-dM-9M-&M-eM-1M-1M-fM-^\M-^IM-hM-7M-/M-eM-^KM-$M-dM-8M-:M-eM->M-^DM-oM-<M-^A$
! 10 #@!$%^&*()|<>?$
! # Completed$
! Empty$
! # To be done$
! 1 $
! 2 $
! 3 foo bar$
! 4 foo bar$
! 5 foo   bar$
! 6    foo   bar   $
! 9 M-dM-9M-&M-eM-1M-1M-fM-^\M-^IM-hM-7M-/M-eM-^KM-$M-dM-8M-:M-eM->M-^DM-oM-<M-^A$
! 10 #@!$%^&*()|<>?$
! # Completed$
! 7 9$
! 8 10$
! # To be done$
! Empty$
! # Completed$
! 1 $
! 2 $
! 3 foo bar$
! 4 foo bar$
! 5 foo   bar$
! 6    foo   bar   $
! 7 9$
! 8 10$
! 9 M-dM-9M-&M-eM-1M-1M-fM-^\M-^IM-hM-7M-/M-eM-^KM-$M-dM-8M-:M-eM->M-^DM-oM-<M-^A$
! 10 #@!$%^&*()|<>?$
--- 1,17 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
```

#### 🔴 Scenario: mark 任务时没有提供任何 ID

Steps:

```bash
rm -rf ~/.todo
todo init
todo add foo
todo add bar
todo mark 2
todo list
todo mark
echo $?
todo list
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,10 ****$
! Initialized successfully.$
! # To be done$
! 1 foo$
! # Completed$
! 2 bar$
! 0$
! # To be done$
! 1 foo$
! # Completed$
! 2 bar$
--- 1,8 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! 1$
! Please run 'todo init' before running 'list' command.$
```

#### 🔴 Scenario: 同时 mark 多个已 completed、不存在及已删除的任务

Steps:

```bash
rm -rf ~/.todo
todo init
todo add foo
todo add bar
todo add foobar
todo list
todo mark 2 3
[[ -e ~/.todo/tasks ]] && cp ~/.todo/tasks /tmp
todo list
todo mark 3 2 404
echo $?
todo list
[[ -e /tmp/tasks ]] && diff --brief /tmp/tasks ~/.todo/tasks
echo $?
```

Output diff: Expected vs Actual

```plaintext
diff --expand-tabs -C 100 ./expected-output.log /tmp/scenario-output.log | cat -vet
*** ./expected-output.log^IMon Jul  4 06:27:49 2022$
--- /tmp/scenario-output.log^IMon Jul  4 12:07:45 2022$
***************$
*** 1,19 ****$
! Initialized successfully.$
! # To be done$
! 1 foo$
! 2 bar$
! 3 foobar$
! # Completed$
! Empty$
! # To be done$
! 1 foo$
! # Completed$
! 2 bar$
! 3 foobar$
! 0$
! # To be done$
! 1 foo$
! # Completed$
! 2 bar$
! 3 foobar$
! 0$
--- 1,11 ----$
! Please run 'todo init' before running 'init' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'add' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! Please run 'todo init' before running 'list' command.$
! Please run 'todo init' before running 'mark' command.$
! 1$
! Please run 'todo init' before running 'list' command.$
! 1$
```

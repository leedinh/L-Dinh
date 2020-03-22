---
title: Angstong Canary Writeup
---
Xin chào mọi người, post này mình sẽ writeup là 1 bài trong Angstrong CTF vừa qua và mình sẽ giới thiệu các bạn về ***stack canary***, kĩ thuật ***format string exploit*** trong BOF chal.... :smiley:

# Stack Canaries
![stackcana](img/ca.png)
Đơn giản, stack canaries là một giá trị được giấu trong stack, để defend trước lỗi BOF.
![disca](img/disca.png)
Khi exc tới đây, program sẽ xor $rax với canary, nếu ZF off thì prog sẽ bị ngắt bởi hàm stack_chk_fail :grimacing:

# Angstrong canary
## Problem
![problem](img/problem.png)
## Source
![source](img/source.png)
## Exploitinggg
### #Checksec
![check](img/checksec.png)
Canary is onn!!!! :grinning:



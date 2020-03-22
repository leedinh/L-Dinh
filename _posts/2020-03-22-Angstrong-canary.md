---
title: Angstrong Canary Writeup
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
### Exec
Chạy thử chương trình... ta thấy prog cho nhập vào ở 2 chỗ bằng hàm **gets** (theo source), đây là nơi BOF xảy ra.
![check](img/exe.png)
Hãy xem thử asm của func greet, hàm stack_chk_fail xuất hiện ở cuối.
![check](img/asm.png)
### Debug
Đầu tiên mình sẽ xem thử nếu chương trình exec bình thường thì sẽ như thế nào.
Breakpoint!!! 
![check](img/break.png)

![check](img/exaa.jpg)

Nhiệm vụ của chúng ta bây giờ có 2 phần:
1. Leak được stack canary để pass được stack_chk_fail.
2. BOF để khiến hàm greet return về hàm >flag<
 
### Stack Canary Leakingg
Nếu để ý, các bạn sẽ thấy hàm printf sẽ trả về những gì mà ta nhập trên hàm gets --> format string exploit.
Nếu ta nhập vào các format string thì nó sẽ trả về những giá trị nằm trên stack hiện tại bao gồm cả **stack canary**

BẮT ĐẦU SPAM THÔI!!
![check](img/spam.png)
![check](img/cc.png)
Khi i=17 thì được 1 string giống format của canary ta chạy thử ở trên >> ta đã leak được stack canary.

Tiếp theo mình sẽ tính offset để đè canary đã leak được lên rax.
![check](img/ofs1.png)
![check](img/ofs2.png)
Vậy là mình đã tính vị trí của canary + 8byte ebp > return address.

### Payload thôiii
![check](img/payload.png)
Ở trên là code python mình dùng pwntools để exploit.
![check](img/flag.png)
TADA!!! chúng ta nhận được flag >...<


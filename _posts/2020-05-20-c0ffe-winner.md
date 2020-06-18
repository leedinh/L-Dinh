---
title: C0ffeeWinner
---
Xin chào các bạn, hôm nay mình sẽ writeup 2 bài mà anh Minh đã giao. 2 bài này mình đã làm khá lâu nhưng do làm biếng nên tới hôm nay mới writeup lại ~(>_<。)＼  

Tuy không khó nhưng 2 bài có dạng khá lạ và mình phải xin hint khá là nhiều (╯▽╰ )

# C0FFEE
## Overview
![pwn1intro](img/prompt.png)
Bài này sẽ cho mình 1 cái prompt mua c0ffe, nhìn vào thì khá là rắc rối @@.  
### Checksec
![pwn1intro](img/c1.png)
Không có ASLR hay Canary, chúng ta có thể nghĩ đến ret2libc.

### Bug
Nhìn qua pseudocode của cả bài ><
![pwn1intro](img/pc1.png)  
![pwn1intro](img/pc2.png)  
Có một số chỗ chúng ta cần chú ý: 
![pwn1intro](img/c3.png)
v9 nằm trên stack là cho phép ta nhập vào 1320 byte là vừa đủ và chương trình sẽ return bình thường.  
Theo pseudocode cả chương trình nằm trong 1 vòng while, khi bạn nhập yes ở hàm scanf cuối cùng. Nhưng tới lần thứ 10 thì chương trình sẽ tự exit.
![pwn1intro](img/c4.png)
![pwn1intro](img/c5.png)
![pwn1intro](img/c6.png)
Mình sẽ gọi tên biến theo ida.   
var548 là biến đếm số lần lập của chương trình và var548 >= v10( v10 max=10 ) thì vòng while sẽ bị ngắt.  
Mỗi lần ta chỉ có thể nhập 132 byte vào v9 ==> 132* 10 tối đa, không thể overflow.  
Nhưng các bạn hãy để ý kĩ dòng scanf thứ 3, không có điều kiện để check user nhập vào =)) nên bof có thể xảy ra tại đây. 
![pwn1intro](img/co1.png)
Nhìn trên stack thì &nptr cách var548(count của vòng while) 8 byte.  

Hàm **scanf("%08s%\*c", &nptr)** có nghĩa là nó sẽ nhận 8 byte vào nptr và *"%\*c"* sẽ discard 1 byte tiếp theo là "\n"(do mình nhập), cuối cùng sẽ auto append null byte vào string "\x00".  


Ta có thể overflow nptr làm nó write null byte vào biến nằm sau nó var548. Vì var548 là biến count mỗi lần lặp đều bị ghi thành 0 nên ta có thể lặp bao nhiêu lần tùy thích. Vậy ta có thể ghi 9 byte vào nptr.  
![pwn1intro](img/co2.png)
Chúng ta sẽ viết payload trong một hàm và sẽ loop bằng vòng for.  
Vậy là xongg!!! Tới đây bài sẽ trở về 1 bài ret2libc bình thường, các bạn có thể gọi sys + bin/sh để exec được shell như các bài trước.
(o゜▽゜)o☆




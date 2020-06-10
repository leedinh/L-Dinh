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

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
Mình sẽ gọi tên biến theo ida, var548 là biến đếm số lần lập của chương trình và var548 >= v10( v10 max=10 ) thì vòng while sẽ bị ngắt.  
Mỗi lần ta chỉ có thể nhập 132 byte vào v9 ==> 132* 10 tối đa, không thể overflow.  
Nhưng các bạn hãy để ý kĩ dòng scanf thứ 3, không có điều kiện để check user nhập vào =)) nên bof có thể xảy ra tại đây. 
![pwn1intro](img/co1.png)
Nhìn trên stack thì &nptr cách var548(count của vòng while) 8 byte.  

Hàm **scanf("%08s%\*c", &nptr)** có nghĩa là nó sẽ nhận 8 byte vào nptr và *"%\*c"* sẽ discard 1 byte tiếp theo là "\n"(do mình nhập), cuối cùng sẽ auto append null byte vào string "\x00".  


## Exploit
Ta có thể overflow nptr làm nó write null byte vào biến nằm sau nó var548.  
Vì var548 là biến count mỗi lần lặp đều bị ghi thành 0 nên ta có thể lặp bao nhiêu lần tùy thích. Vậy ta có thể ghi 9 byte vào nptr.  
![pwn1intro](img/co2.png)
Chúng ta sẽ viết payload trong một hàm và sẽ loop bằng vòng for.  

Vậy là xongg!!! Tới đây bài sẽ trở về 1 bài ret2libc bình thường, các bạn có thể gọi sys + bin/sh để exec được shell như các bài trước.
(o゜▽゜)o☆


# WINNER
Bài này thì anh Minh nói dễ hơn bài trên nhưng vẫn rất là khó hiểu @@.

## Overview
![pwn1intro](img/wi1.png)
Prompt cho bạn nhập vào 1 chuỗi ký tự kết thúc bằng 0 và in ra số tiền bạn thắng.
Ta sẽ xem pseudocode của hàm nhập vào.
![pwn1intro](img/wi2.png)
Bài sử dụng hàm getchar() để cho bạn nhập vào 1 ký tự khi c='0' thì hàm exit. Nhìn thì cũng không có gì đặc biệt.
## Bug + Exploit
Ta có thể thấy getchar() là hàm nhập vào duy nhất nên có thể là bug sẽ xảy ra ở đây.
Chường trình cho chúng ta nhập vào 1 byte ->(0x00-0xff). Ta có thể thử nhập từ 0x00-0x7f đây là những char dương (c>0) và sẽ không đủ length để overflow tới ret address.

Sẽ ra sao nếu chúng ta nhập c<0 0x80-0xff. Ta có thể lợi dụng 1 đoạn asm để overflow ret address @@ mã gì tuốt trong asm.
![pwn1intro](img/wi3.png)

Trước hết các bạn hãy đặt bp tại 0x08048900, đây là đoạn asm ta dùng để overflow ret addr.  
Ta thấy **mov DWORD PTR [edx+ecx\*4],eax** có nghĩa là tại address ($edx+$ecx\*4) sẽ gán bằng $eax. Vì ta thể tùy chỉnh $ecx, do đây là register lưu byte ta nhập vào. Ví dụ nếu mình nhập vào 0x90, hãy quan sát $eax, $ecx, $edx.
![pwn1intro](img/wi4.png) 
$ecx sẽ có dạng 0xffffff90 vì đây là 1 signed char và 0x90 <0. 
Nhưng đây chỉ là address ở máy mình có thể khác ở máy mọi người nhưng offset là giống nhau.
Ta có thể tùy chỉnh ecx để *$edx+$ecx\*4*= $ebp-0x4. À mà bài này các bạn chỉnh cần chỉnh ret address thành address của hàm flag là có thể lấy được flag.

### Compute
Mình sẽ lấy $ebp-0x4 và $edx để tính ngược lại $ecx ta cần nhập.
![pwn1intro](img/wi5.png)
Công thức cũng đơn giản thôi $ecx=($ebp+0x4-$ecd)/4. Và ta có được kết quả là 0xe1
Mỗi lần lặp thì eax được inc(1), nên ta sẽ tính từ 0x08048a65 tới address của chỗ readfile flag 0x8048B13 là bao nhiêu, đơn giản là trừ ra offset=217. Vậy là xong!!! 
![pwn1intro](img/wi6.png)

# Kết
Vậy là xong 2 bài của anh Minh giao cho mình, vẫn còn 1 bài về null byte nữa mình sẽ tiếp tục writeup bài đó ＞︿＜
À các bạn có thể lấy 2 bài này tại đây nha >[(╹ڡ╹ )](https://github.com/leedinh/InfoSec/tree/master/c0ffexwinner)<





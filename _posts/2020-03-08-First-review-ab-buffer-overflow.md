---
title: Buffer Overflow
---


Hii, đây là post đầu tiên để write up những gì mình hiểu về buffer overflow exploitation, một common chal khi mình bắt đầu chơi pwn, và mình cũng đang nghiên cứu sâu hơn về nó. mình đang học theo series [binary exploitation](https://www.youtube.com/watch?v=iyAyN3GFM7A&list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN) của LiveOverflow được anh Minh recommend, là 1 series cơ bản cho pwn dummies, mình thấy kênh này hướng dẫn khá là dễ hiểu, btw mình cũng chỉ mới bắt đầu chơi pwn có mấy tháng nên mình gà zlll =))))

  
# Một số thứ mới mà mình học được
## Stuffs
  
Mình học cũng được kha khá thứ tuy là basic với người ta nhưng là mới đối với mình. Trước tiên, mình đã dần quen hơn với việc làm       việc trên CLI, mình biết được nhiều command hơn, nói chứ xài linux hay mac lúc nào cũng ngầu hơn nhờ có mấy cái terminal này =)))         không lằng nhằng như trên window. Ngoài ra, mình còn học được 1 ít code python, đọc được ASM xài được GDB. Tài liệu thêm mà mình           tham khảo là cuốn [Hacking the art of exploitation](https://github.com/leedinh/CyberSec/blob/master/Hacking-%20The%20Art%20of%20Exploitation%20(2nd%20ed.%202008)%20-%20Erickson.pdf).

## Stack

Stack là một vùng nhớ được tạo ra để chứa các biến, mỗi lần function call nó sẽ tạo ra một vùng nhớ được gọi là stack frame trên stack.
Stack phân bộ từ địa chỉ từ lớn tới nhỏ, stack gồm những registers chứa địa chỉ: register ESP(Stack Pointer) chứa address của giá trị trên cùng của stack (stack top). Stack là một cấu trúc FILO ta có thể truy xuất các giá trị nhờ ESP.

Ngoài ra trong stack còn có register EIP(Intruction Pointer), register chỉ dẫn chứa các intructions hướng dẫn chương trình exc. Mỗi khi function call, được đẩy vào stack là register EBP(Base Pointer), nó là save point của 1 stack frame và cố định trước khi function được return. Các registers khác như eax, ebx được dùng để chứa các giá trị thực hiện tính toán trong khi function call.
                        ![stack](img/stack.png)


# Kết
Đó là một số thứ mình đã học được, cũng như là hiểu biết của bản thân mình về stack =3, tạm thời chỉ có nhiêu đây, mình sẽ tiếp tục nghiên cứu thêm và chỉnh sửa cho các bài post sắp tới.




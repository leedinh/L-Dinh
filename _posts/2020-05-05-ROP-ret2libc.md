---
title: First Rop
---
Xin chào mọi người, post hôm nay mình sẽ viết về chal ROP đầu tiên của mình, chal này mình được anh Minh giao, lấy từ Midnight Sun CTF 2020-**PWN1**<br />
( •̀ ω •́ )✧

# PWN1
![pwn1intro](img/intro.png)

## Checksec&file
![pwn1intro](img/filesec.png)
Do remote có bật aslr nên mình cũng sẽ bật aslr ở máy mình.
Vì ASLR và NX được bật nên đây rõ ràng là 1 rop chal.

## Disas
Coi sơ qua file binary bằng ida, hàm main được viết khá đơn giản:
![pwn1intro](img/pesudo.png)
![pwn1intro](img/disas.png)
### Bug!!
Có thể dễ dàng nhận ra bug ở hàm gets dẫn đến lỗi BOF (⊙_⊙)？.
Ta có thể dễ dàng tìm được ret offset của prog..
![pwn1intro](img/offset1.png)
![pwn1intro](img/offset2.png)

## Exploit
Ý tưởng ở đây mình sẽ ret2libc bằng exc system + "/bin/sh", nhưng do alsr được bật nên mỗi lần run thì address của sys + "/bin/sh" sẽ thay đổi. Thế nên ta phải leak được libc-base, vì address của các sym trong libc đều sẽ bằng libc_base(thay đổi)+ offset(cố định).

Ta có thể tính được bằng cách sử dụng hàm puts để in address của chính hàm puts trên GOT =)) sau đó trừ với offset của hàm puts trong libc là ra được libc_base.

## Leaking libc
Trước hết mình sẽ tạo ra 1 rop chain để leak puts.got (～￣▽￣)～<br />

Ta cần 1 gadget để ghi addr của puts.got sau đó ret về puts.plt với cách này thì ta có thể in ra arbitrary address. Mình sẽ sử dụng gadget **pop $rdi ; ret**.<br />

Layout của ropchain sẽ như thế này:<br />
rop1=padding(72 byte) + pop_rdi_gadget + puts.got + puts.plt + main_address( ((⊙﹏⊙))o.).

### pop $rdi ; ret
![pwn1intro](img/rdi.png)
Dùng ROPgadget để lấy được address.
Ta được rop chain đầu tiên
![pwn1intro](img/rop1.png)
### Leaking
![pwn1intro](img/leak.png)
![pwn1intro](img/leaked.png)
Address được xuất ra là của got_puts, ta có thể dễ dàng tìm được libc_base= got_puts - put_libc_offset.

## Ret2libc
Cuối cùng ta đã có được libc address, việc còn lại là từ libc_base ta sẽ tính được addr của sys và "/bin/sh":
![pwn1intro](img/sys.png)

### ROP2
Mình sẽ viết thêm 1 rop chain nữa bể sys call "/bin/sh" hoặc có thể sử dụng one_gadget cho nhanh.

Layout của rop2:<br />
rop2=payload(72) + pop_rdi_gadget + sh_address + sys + main
![pwn1intro](img/rop2.png)
Thế là đã xong, giờ chỉ cần send code lên remote là sẽ ra flag (*￣3￣)╭
![pwn1intro](img/last.png)
Tada (●ˇ∀ˇ●)

Full expl.py [full exploition](https://github.com/leedinh/InfoSec/tree/master/ROP/pwn1_git)

Chào tạm biệt mọi người ( •̀ ω •́ )y










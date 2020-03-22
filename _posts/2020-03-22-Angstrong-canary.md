---
title: ANGSTRONG CANARY WRITEUP
---
Xin chào mọi người, post này mình sẽ writeup là 1 bài trong Angstrong CTF vừa qua và mình sẽ giới thiệu các bạn về ***stack canary***, kĩ thuật ***format string exploit*** trong BOF chal.... :relaxed:  

# Stack Canaries
![stackcana](img/stackca.png)
Đơn giản là stack canaries là một giá trị được giấu trong stack, để defend trước lỗi BOF.
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


h1 {
  color: #ffaa33;
  font-size: 1.5em;
  }

{% endhighlight %}

And this is a HTML example, with a linenumber:
{% highlight html linenos %}

<html>
  <a href="example.com">Example</a>
</html>

{% endhighlight %}

Last, a Ruby example:
{% highlight ruby linenos %}

def hello
  puts "Hello World!"
end

{% endhighlight %}

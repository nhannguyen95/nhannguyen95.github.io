---
layout: post
title:  "Syntax Highlighting Test"
date:   2017-03-24 01:30:13 +0800
categories: default
tags: test syntax
---
Math equation test:
Nhận xét rằng các điểm nằm về cùng 1 phía so với đường thẳng này sẽ làm cho hàm số
\\(f_{\mathbf{w}}(\mathbf{x})\\) mang cùng dấu. Chỉ cần đổi dấu của \\(\mathbf{w}\\) nếu cần thiết, ta có thể giả sử các điểm nằm trong nửa mặt phẳng nền xanh mang dấu dương (+), các điểm nằm trong nửa mặt phẳng nền đỏ mang dấu âm (-). Các dấu này cũng tương đương với nhãn \\(y\\) của mỗi class. Vậy nếu \\(\mathbf{w}\\) là một nghiệm của bài toán Perceptron, với một điểm dữ liệu mới \\(\mathbf{x}\\) chưa được gán nhãn, ta có thể xác định class của nó bằng phép toán đơn giản như sau:
\\[
\text{label}(\mathbf{x}) = 1 ~\text{if}~ \mathbf{w}^T\mathbf{x} \geq 0, \text{otherwise} -1
\\]

Ngắn gọn hơn:
\\[
\text{label}(\mathbf{x}) = \text{sgn}(\mathbf{w}^T\mathbf{x})
\\]
trong đó, \\(\text{sgn}\\) là hàm xác định dấu, với giả sử rằng \\(\text{sgn}(0) = 1\\).

Jekyll uses Rouge by default for syntax highlighting, here are some tests.

Ruby:
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Python with line numbers:
{% highlight python linenos %}
def print_hi(name):
    print("Hi, {}".format(name))

print_hi('Tom')
# prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

C with line numbers:
{% highlight c linenos %}
void print_hi(string name) {
  printf("Hi, %s", name);
}
print_hi("Tom");
/* prints 'Hi, Tom' to STDOUT. */
{% endhighlight %}

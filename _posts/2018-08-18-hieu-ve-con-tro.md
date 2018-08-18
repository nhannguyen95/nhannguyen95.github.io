---
layout: post
title: "Hiểu về con trỏ trong C/C++"
date: 2018-07-20 20:48:00 +0700
categories: [programming languages]
tags:
  - c/c++
comments: true
mathjax: true
content_level: 1
img:
summary: Hiểu thêm về con trỏ trong C/C++.
---

Trước khi vào vấn đề, định nghĩa một cấu trúc "node" trước:

{% highlight cpp linenos %}
struct ListNode {
  int value;
  ListNode* next;
  ListNode(x): value(x), next(NULL) {}
};
{% endhighlight %}

Với cấu trúc trên, chúng ta có thể tạo ra 3 nodes như thế này:

{% highlight cpp linenos %}
ListNode* head = new ListNode(1);
ListNode* node2 = new ListNode(2);
ListNode* node3 = new ListNode(3);
{% endhighlight %}

Cùng giải thích rõ hơn ý nghĩa của lệnh gán trên:
- Toán tử `new ListNode(1)` sẽ cấp phát bộ nhớ trên _HEAP_, và trả về một giá trị là địa chỉ của vùng nhớ này.
- `ListNode* head`: khai báo một biến con trỏ trỏ đến kiểu dữ liệu `ListNode`, biến con trỏ này được lưu trên bộ nhớ _STACK_.
- Lệnh gán `ListNode* head = new ListNode(1)`: gán một giá trị địa chỉ (của bộ nhớ) cho một con trỏ; lệnh gán liên quan đến con trỏ thường có dạng vế trái là một con trỏ, vế phải là địa chỉ X của một vùng nhớ nào đó; như vậy lệnh gán có thể được hiểu là: **con trỏ `head` trỏ đến vùng nhớ có địa chỉ là X, vùng nhớ này lưu `ListNode(1)`.**

Hình vẽ minh hoạ:

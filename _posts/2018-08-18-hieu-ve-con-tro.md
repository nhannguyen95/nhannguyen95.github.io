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

{% include image.html
  url="/images/pointer_1.jpg"
  cap="Hình 1. Cách các nodes được cấp phát trên bộ nhớ."
%}

Tiếp theo, liên kết 3 nodes này lại với nhau để tạo thành danh sách liên kết đơn:

{% highlight cpp linenos %}
head->next = node2;
node2->next = node3;
{% endhighlight %}

Các lệnh gán trên cũng được giải thích hoàn toàn tương tự (chúng ta sẽ nhận thấy rằng về bản chất lệnh này với lệnh cấp phát bên trên là một):
- `head` là biến con trỏ trên _STACK_,  con trỏ này trỏ đến bộ nhớ trên _HEAP_ thuộc kiểu `ListNode` chứa biến `value=1` và `next=NULL`, vậy `head->next` chính là biến con trỏ next trên _HEAP_.
- Lệnh `head->next = node2` có vế trái là một con trỏ, vế phải là địa chỉ của vùng nhớ trên _HEAP_ và `node2` đang trỏ tới (`value=2` và `next=NULL`). Như phân tích ở trên, lệnh gán này sẽ làm con trỏ `head-next` trỏ tới vùng nhớ có địa chỉ `node2`.

Phân tích tương tự cho lệnh con lại, ta có hình minh hoạ sau đây:


{% include image.html
  url="/images/pointer_2.jpg"
  cap="Hình 2. Cách các nodes liên kết tạo thành danh sách đơn."
%}

Happy coding,

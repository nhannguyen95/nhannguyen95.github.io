---
layout: post
title: "Hiểu về con trỏ trong C/C++ phần 2"
date: 2018-07-20 13:49:00 +0700
categories: [programming languages]
tags:
  - c/c++
comments: true
mathjax: true
content_level: 1
img:
summary: Hiểu thêm về con trỏ trong C/C++ phần 2.
---


Trích đoạn câu trả lời của [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) trên [Slashdot Interview](http://meta.slashdot.org/story/12/10/11/0030249/linus-torvalds-answers-your-questions):

>> I've seen too many people who delete a singly-linked list entry by keeping track of the "prev" entry, and then to delete the entry, doing something like:

{% highlight cpp linenos %}
if (prev)
	prev->next = entry->next;
else
	head = entry->next;
{% endhighlight %}

and whenever I see code like that, I just go "This person doesn't understand pointers". And it's sadly quite common. People who understand pointers just use a "pointer to the entry pointer", and initialize that with the address of the list_head. And then as they traverse the list, they can remove the entry without using any conditionals, by just doing:

{% highlight cpp linenos %}
*pp = entry->next;
{% endhighlight %}

Trong bài này chúng ta sẽ tìm hiểu solution mà Linus đã đề xuất. Trước tiên hãy cùng code hàm xoá một node trong một danh sách liên kết (dslk) đơn mà đa số mọi người thường viết như sau:

{% highlight cpp linenos %}
// Delete `target` node from a singly linked list whose head is `head`.
void deleteNode(ListNode*& head, ListNode* target) {
	ListNode* entry = head;
	ListNode* prev = NULL;

	while(entry) {
		if (entry == target) {
			if (prev) {
				prev->next = entry->next;
			} else {
				head = entry->next;
			}
			break;
		}

		prev = entry;
		entry = entry->next;
	}
	
	delete target;
}
{% endhighlight %}

Giải thích một chút về việc truyền `head` bằng tham chiếu: nếu tham số `head` không được truyền bằng tham chiếu, thì chúng ta sẽ chỉ xoá được những nodes không phải là `head` (vì câu lệnh `entry = entry->next` lần đầu tiên - tức là `entry = head->next` giúp entry duyệt trên chính các con trỏ được cấp phát trên _HEAP_), chứ không xử lý được trường hợp nodes được xoá là `head`, vì lúc này con trỏ `head` ở trong hàm và ở ngoài hàm là hoàn toàn khác nhau, trường hợp `target = head`, câu lệnh `head = target->next` sẽ được chạy. Câu lệnh này thứ nhất, nó chỉ làm thay đổi con trỏ `head` ở trong hàm mà thôi (con trỏ `head` ngoài hàm không ảnh hưởng gì); thứ 2, nó chỉ thay đổi hướng trỏ của con trỏ `head` này sang bộ nhớ tại `target->next`, chứ không thực sự thay đổi ở con trỏ nằm trên bộ nhớ _HEAP_ mà con trỏ `head` ban đầu trỏ tới. Nói tóm lại, ta truyền tham số `head` bằng tham chiếu để nếu `head` bên trong hàm thay đổi thì `head` ngoài hàm cũng thay đổi.

Cách code trên không tối ưu ở chỗ nếu muốn xoá một node, chúng ta cần biết node trước đó của nó, riêng `head` thì không có node trước đó nên trường hợp đặc biệt này phải luôn được xét. Linus Torvalds muốn tránh viết thêm code cho trường hợp đặc biệt này: dùng con trỏ cấp 2 để xác định node liền trước của mọi node, kể cả `head`. Để hiểu được cách làm này, trước hết cần phải hiểu rằng giả sử dslk có n node, thì n node này sẽ nằm ở _HEAP_ (tại n vùng nhớ); mỗi node này đếu chứa con trỏ `next` trỏ đến vùng nhớ của node tiếp theo. Vậy con trỏ nào trỏ đến node đầu tiên? Đó là `head`! Và nó nằm trên _STACK_:

{% include image.html
  url="/images/pointer_3.jpg"
  cap="Hình 1. Minh hoạ bộ nhớ thời điểm ban đầu."
%}

Đầu tiên, khởi tạo con trỏ `entry` trỏ đến node đầu tiên của danh sách:

{% highlight cpp linenos %}
ListNode* entry = head;
{% endif %}

Tương tự như thuật toán đầu tiên, `entry` sẽ duyệt qua toàn bộ node của dslk, và ta cần biết node liền trước của  node mà `entry` đang trỏ đến tại mọi thời điểm. Khi `entry` trỏ đến node đầu tiên, ta có thể xem con trỏ `head` đang nằm trên _STACK_ cũng là một node của dslk và lúc này nó chính là node liền trước của node mà `entry` đang trỏ đến. Một cách tự nhiên, ta cần một con trỏ trỏ đến con trỏ `head` này - một con trỏ cấp 2 (pointer to pointer):

{% highlight cpp linenos %}
ListNode** pp = &head;
{% endif %}

Minh hoạ bộ nhớ tại thời điểm hiện tại:

{% include image.html
  url="/images/pointer_4.jpg"
  cap="Hình 2. Minh hoạ bộ nhớ khi có `entry` và `pp`."
%}

Lúc này, chúng ta sẽ không còn lo đến trường hợp node cần xoá là `head` nữa, vì đã luôn luôn xác định được node liền trước cuả mọi node, phần code còn lại hoàn toàn tương tự như thuật toán đầu tiên, code đầy đủ của thuật toán mới:

{% highlight cpp linenos %}
// Delete `target` node from a singly linked list whose head is `head`.
void deleteNode(ListNode*& head, ListNode* target) {
    ListNode** pp = &head;
    ListNode* entry = head;

    while(entry) {
      if (entry == target) {
        *pp = entry->next;
        break;
      }
      pp = &(entry->next);
      entry = entry->next;
    }
    delete target;
}
{% endhighlight %}

Và thậm chí có thể không sử dụng `entry`:

{% highlight cpp linenos %}
// Delete `target` node from a singly linked list whose head is `head`.
void deleteNode(ListNode*& head, ListNode* target) {
    ListNode** pp = &head;
    while(*pp != target) {
      pp = &((*pp)->next);
    }
    *pp = target->next;
    delete target;
}
{% endhighlight %}

Happy coding,

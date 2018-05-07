---
layout: post
title: "The Tortoise and Hare algorithm"
date: 2018-05-03 21:32:00 +0200
categories: ['algorithm']
tags:
  - ad-hoc
comments: true
mathjax: true
img:
summary: Algorithm for finding cycle or duplicate in a sequence.
---

Mình đang khá bận viết báo cáo tốt nghiệp và hoàn thành đồ án các thứ nên sẽ viết free style, who cares anyway?

Cho một singly linked list, bài toán đặt ra là kiểm tra xem linked này có cycle hay không? Nếu có thì tìm phần tử bắt đầu cycle $ \mu$ và độ dài $\lambda$ của cycle.

{% include image.html
  url="/images/cycle_linked_list.jpg"
  cap="Hình 1. Một singly linked list với $\mu = x_2$ và $\lambda = 5$"
%}

Hiểu ngầm rằng $x_7 = x_2, x_8 = x_3$ ... và cứ như thế.

Cần lưu ý rằng $\mu$ không tồn tại khi $x_6$ "vòng về" head của linked list là $x_0$, tức là phải có một vài phần tử ban đầu trước khi linked list hình thành cycle (như $x_0, x_1$).

Thuật toán thỏ và rùa đúng như tên gọi: có 2 pointers tượng trưng cho thỏ và rùa, cùng xuất phát từ một điểm. Thỏ chạy nhanh hơn rùa: khi rùa đi được 1 bước thì thỏ đi được $s (s > 1)$ bước.

Giả sử 2 pointers rùa và thỏ cùng xuất phát từ một $x_i$ nào đó, sau khi rùa đi được $j$ bước và dừng ở $x_{i+j}$, thỏ sẽ đi được $sj$ bước và dừng ở $x_{i+sj}$.

Nếu linked list không có cycle, sau vài bước nào đó pointer thỏ sẽ `NULL`.

Nếu có cycle, ta hi vọng rằng sau vài bước thỏ và rùa sẽ gặp nhau tại một phần tử nào đó (phần tử này đương nhiên phải thuộc cycle).

Giả sử sau $j$ bước thỏ và rùa gặp nhau, tức là $x_{i+j} = x_{i+sj}$.

Điều này xảy ra khi $x_{i+j}$ và $x_{i+sj}$ cách nhau một số nguyên lần độ dài $\lambda$ của cycle, tức là:

$$
(i+sj) - (i+j) = k\lambda \\
j = \frac{k\lambda}{s-1}
$$

Dễ thấy với $s = 2$ luôn thoả mãn phương trình trên với mọi $j, k, \lambda$.

Tức là: **Nếu rùa và thỏ xuất phát cùng một điểm và cứ rùa đi một bước thỏ đi 2 bước, thì chắc chắn sau một số bước hữu hạn nào đấy sẽ gặp nhau.**

Ok, bây giờ đi tìm $\mu$ trước. Để dễ hình dung thì với $s=2$, và giả sử 2 pointers xuất phát ở $x_0$, thì sẽ gặp nhau ở $x_5$.

Lúc này rùa đang ở $x_{i+j} = x_{i+k\lambda}$ (tức $x_5$), ta reset thỏ về vị trí xuất phát là $x_{i}$ (tức $x_0$) _(cần chú ý rằng $x_i$ phải là phần tử nằm ngoài cycle thì mới tìm được $\mu$)_.

$x_i$ và $x_{i+k\lambda}$ cách nhau một số nguyên lần độ dài của cycle; nhưng $x_i \neg x_{i+k\lambda}$ vì một phần tử thuộc cycle, phần tử kia thì không. Bằng cách di chuyển 2 con trỏ đồng thời để duy trì khoảng cách $k\lambda$, thì ngay khi con trỏ thỏ bước vào cycle, nó sẽ gặp rùa ngay tại phần tử bắt đầu cycle - $\mu$.

Quay trở lại ví dụ minh hoạ: thỏ ở $x_0$, rùa ở $x_5$; sau đó cả hai tiến đến $x_1$, $x_6$; thêm bước nữa sẽ gặp nhau ở $x_2$; vậy $x_2$ là phần tử bắt đầu cycle.

Để tìm $\lambda$ thì đơn giản: phẩn tử đầu tiên đã được xác định, cho một con trỏ chạy một vòng quanh cycle để đếm độ dài là xong.

Code C++ chưa handle trường hợp không cycle (độc giả tự hoàn thành):

{% highlight c++ linenos %}
void detectCycle(Node * x0) {
  Node * tho = x0, * rua = x0;
  do {
    rua = rua->next;
    tho = tho->next->next;
  } while(rua != tho)

  // Find mu
  while(rua != tho) {
    rua = rua->next;
    tho = tho->next;
  }
  // Now rua (or tho) is mu

  // Find lambda
  int lambda = 0;
  do {
    lambda++;
    tho = tho->next;
  } while(tho != rua)
}
{% endhighlight %}

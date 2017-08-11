---
layout: post
title:  "Bài tập xây dựng cây quyết định sử dụng thuật toán ID3"
date: 2017-08-11 17:19:00 +0700
categories: ['data mining']
tags:
no-post-nav: 0
comments: 1
---

##### **Bài toán**

Sử dụng thuật toán ID3, xây dựng cây quyết định cho tập dữ liệu huấn luyện sau:

|Day|Outlook|Temp|Humidity|Wind|PlayTennis|
|-|-|-|-|-|-|
|1|Sunny|Hot|High|Weak|No|
|2|Sunny|Hot|High|Strong|No|
|3|Overcast|Hot|High|Weak|Yes|
|4|Rain|Mild|High|Weak|Yes|
|5|Rain|Cool|Normal|Weak|Yes|
|6|Rain|Cool|Normal|Strong|No|
|7|Overcast|Cool|Normal|Strong|Yes|
|8|Sunny|Mild|High|Weak|No|
|9|Sunny|Cool|Normal|Weak|Yes|
|10|Rain|Mild|Normal|Weak|Yes|
|11|Sunny|Mild|Normal|Strong|Yes|
|12|Overcast|Mild|High|Strong|Yes|
|13|Overcast|Hot|Normal|Weak|Yes|
|14|Rain|Mild|High|Strong|No|

##### **Lời giải**

**Chọn nút gốc của cây quyết định**:

Tập dữ liệu hiện tại có 9 kết quả Yes và 5 kết quả No, ta kí hiệu là S: [9+, 5-].

Theo công thức tính **Entropy** (độ hỗn tạp dữ liệu) của một tập:

$$
Entropy(S) = -p_{\oplus}log_{2}p_{\oplus} - p_{\ominus}log_{2}p_{\ominus }
$$

trong đó:
* $p_{\oplus}$ là tỷ lệ các mẫu thuộc lớp dương trong S.
* $p_{\ominus}$ là tỷ lệ các mẫu thuộc lớp âm trong S.

Tổng quát, nếu có c lớp trong tập S:

$$
Entropy(S) = \sum_{i=1}^{c}-p_{i}log_{2}p_{i}
$$

Vậy:

$$
Entropy(S) = Entropy([9+, 5-]) = -\frac{9}{14}log_{2}\frac{9}{14} - \frac{5}{14}log_{2}\frac{5}{14} = 0.94
$$

**Lưu ý**:
* Entropy là 0 nếu tất cả các thành viên của S đều thuộc về cùng một lớp.
* Entropy là 1 nếu tập hợp chứa số lượng bằng nhau các thành viên thuộc lớp âm và dương.

Xét thuộc tính `Outlook`, thuộc tính này nhận 3 giá trị là Sunny, Overcast, Rain. Ứng với mỗi thuộc tính, ta có:
* S<sub>Sunny</sub>: [2+, 3-] (có nghĩa là trong tập dữ liệu hiện tại (S), có 2 kết quả Yes và 3 kết quả No tại Outlook = Sunny). Tương tự:
* S<sub>Overcast</sub>: [4+, 0-].
* S<sub>Rain</sub>: [3+, 2-].

Tiếp theo tính **Information Gain** (độ lợi thông tin) của thuộc tính `Outlook` trên tập S. Thông số này phản ánh mức độ hiệu quả của một thuộc tính trong phân lớp. Đó là sự rút giảm mong muốn của Entropy gây ra bởi sự phân hoạch các mẫu dữ liệu theo thuộc tính này. Công thức tính IG của thuộc tính A trên tập S như sau:

$$
Gain(S, A) = Entropy(S) - \sum_{v\in Value(A)}\frac{|S_{v}|}{|S|}Entropy(S_{v})
$$

trong đó:
* Value(A) là tập các giá trị có thể cho thuộc tính A.
* S<sub>v</sub> là tập con của S mà A nhận giá trị v.

_Lấy ví dụ với thuộc tính A = `Outlook`, ta có Value(A) = {Sunny, Overcast, Rain}, và S<sub>Sunny</sub> = [2+, 3-] như đã tính ở trên_

Từ công thức, dễ dàng tính được:

$$
Gain(S, Outlook) = Entropy([9+, 5-]) - \sum_{v\in Value(Outlook)}\frac{|S_{v}|}{14}Entropy(S_{v}) = 0.246
$$

Hoàn toàn tương tự, tính được Information Gain cho 3 thuộc tính còn lại:

$$
Gain(S, Temp) = 0.029
$$

$$
Gain(S, Humidity) = 0.152
$$

$$
Gain(S, Wind) = 0.048
$$

Thuộc tính `Outlook` có Information Gain cao nhất, chọn nó làm nút gốc.

<hr>
<center><img src="http://i.imgur.com/r2LMhyG.png"/>
</center>
<center>Hình 1. Cây quyết định hiện tại</center>
<hr>
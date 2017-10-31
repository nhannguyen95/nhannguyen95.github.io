---
layout: post
title:  "Bài tập tiền xử lý dữ liệu"
date: 2017-08-11 16:41:00 +0700
categories: ['data mining']
tags:
no-post-nav: 0
comments: 1
---

##### **Bài toán**
Tiền xử lý tập dữ liệu sau sử dụng kỹ thuật **binning**:

[16 17 20 21 22 23 23 25 28 28 33 34 36 37]

##### **Lời giải**

**Tạo Bin dữ liệu**:

Quan sát rằng tập dữ liệu thuộc đoạn [16, 37]; 37 - 16 = 21. Nếu dựa vào tiêu chí độ rộng bằng nhau (_binning by equal width_), có thể chia làm 7 bin với độ rộng mỗi bin là 3:
* Bin 1 [16, 19): gồm 2 phần tử {16, 17}.
* Bin 2: [19, 22): gồm 2 phần tử {20, 21}.
* Bin 3: [22, 25): gồm 3 phần tử {22, 23, 23}.
* Bin 4: [25, 28): gồm 1 phần tử {25}.
* Bin 5: [28, 31): gồm 2 phần tử {28, 28}.
* Bin 6: [31, 34): gồm 1 phần tử {33}.
* Bin 7: [34, 37]: gồm 2 phần tử {34, 36, 37}.

**Làm trơn các bin**:

Làm trơn theo giá trị trung bình: giá trị trung bình các phần tử trong bin 1 là 16.5, như vậy bin 1 có 2 phần tử {16.5, 16.5}, tương tự:
* Bin 2: [19, 22): gồm 2 phần tử {20.5, 20.5}.
* Bin 3: [22, 25): gồm 3 phần tử {22.6, 22.6, 22.6}.
* Bin 4: [25, 28): gồm 1 phần tử {25}.
* Bin 5: [28, 31): gồm 2 phần tử {28, 28}.
* Bin 6: [31, 34): gồm 1 phần tử {33}.
* Bin 7: [34, 37]: gồm 2 phần tử {35.6, 35.6, 35.6}.

Làm trơn theo trung vị: có thể xem cách tính trung vị tại [đây](https://nhannguyen95.github.io/2017/08/11/cac-dai-luong-do-do-tap-trung-va-phan-tan):
* Bin 1 [16, 19): gồm 2 phần tử {16.5, 16.5}.
* Bin 2: [19, 22): gồm 2 phần tử {20.5, 20.5}.
* Bin 3: [22, 25): gồm 3 phần tử {23, 23, 23}.
* Bin 4: [25, 28): gồm 1 phần tử {25}.
* Bin 5: [28, 31): gồm 2 phần tử {28, 28}.
* Bin 6: [31, 34): gồm 1 phần tử {33}.
* Bin 7: [34, 37]: gồm 2 phần tử {36, 36, 36}.

Làm trơn theo giá trị biên: thay đổi giá trị hiện tại của phần tử đến biên gần nhất, bin 1 có biên là 16 và 17, như vậy bin 1 có 2 phần tử là {16, 17}, tương tự:
* Bin 1 [16, 19): gồm 2 phần tử {16, 17}.
* Bin 2: [19, 22): gồm 2 phần tử {20, 21}.
* Bin 3: [22, 25): gồm 3 phần tử {22, 23, 23}.
* Bin 4: [25, 28): gồm 1 phần tử {25}.
* Bin 5: [28, 31): gồm 2 phần tử {28, 28}.
* Bin 6: [31, 34): gồm 1 phần tử {33}.
* Bin 7: [34, 37]: gồm 2 phần tử {34, 37, 37}.

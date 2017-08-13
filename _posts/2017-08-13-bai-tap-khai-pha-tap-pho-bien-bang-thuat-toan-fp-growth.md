---
layout: post
title:  "Bài tập khai phá tập phổ biến bằng thuật toán FP-Growth"
date: 2017-08-13 14:58:00 +0700
categories: ['data mining']
tags:
no-post-nav: 0
comments: 1
---

##### **Bài toán**

Cho bảng dữ liệu bao gồm các giao dịch (tid) sau:

|TID|Items|
|-|-|
|1|f, a, c, d, g, i, m, p|
|2|a, b, c, f, l, m, o|
|3|b, f, h, j, o|
|4|b, c, k, s, p|
|5|a, f, c, e, l, p, m, n|

Tìm các tập mục có độ hỗ trợ ≥ 0.6 (tức tần số sup. ≥ 3).

##### **Lý thuyết**

Xem thêm: [bài tập khai phá tập phổ biến bằng thuật toán Apriori](https://nhannguyen95.github.io/2017/08/12/bai-tap-khai-pha-tap-pho-bien-bang-thuat-toan-apriori).

FP-Growth biểu diễn dữ liệu các giao dịch bằng một cấu trúc dữ liệu gọi là FP-Tree.

FP-Growth sử dụng FP-Tree để xác định trực tiếp các tập hạng mục phổ biến (không sinh các tập hạng mục ứng viên từ các tập hạng mục ứng viên trước).

Khi một FP-Tree đã được xây dựng, FP-Growth sử dụng cách tiếp cận chia để trị đệ quy để khai thác các tập phổ biến.

Với mỗi giao dịch, FP-Tree xây dựng một đường đi (path) trong cây.

_Hai giao dịch có chứa cùng một số các mục, thì đường đi của chúng sẽ có phần (đoạn) chung._

_Càng nhiều các đường đi có phần tử chung, thì việc biểu diễn bằng FP-Tree sẽ càng gọn (compressed/compacted)._

**Xây dựng FP-Tree**

Ban đầu, FP-Tree chỉ chứa duy nhất nút gốc (được biểu diễn bởi ký hiệu `null`).

Cơ sỡ dữ liệu các giao dịch được duyệt lần 1, để xác định độ hỗ trợ của mỗi mục.
* Các mục không thường xuyên _(infrequent items)_ bị loại bỏ.
* Các mục thường xuyên _(frequent items)_ được sắp xếp theo thứ tự giảm dần về độ hỗ trợ.

Cơ dở dữ liệu các giao dịch được duyệt lần thứ 2, để xây dựng FP-Tree.

Để hiểu rõ hơn thuật toán, xem lời giải dưới đây.

##### **Lời giải**

Đầu tiên tìm các item mức 1 có sup. ≥ 3, và sắp xếp theo thứ tự giảm dần:

|items|sup.|
|-|-|
|f|4|
|c|4|
|a|3|
|b|3|
|m|3|
|p|3|

Tiếp theo sắp xếp các mục phổ biến mức 1 vừa tìm được theo thứ tự giảm dần trong mỗi giao dịch:

|TID|Items|Items phổ biến|
|-|-|-|
|1|f, a, c, d, g, i, m, p|f, c, a, m, p|
|2|a, b, c, f, l, m, o|f, c, a, b, m|
|3|b, f, h, j, o|f, b|
|4|b, c, k, s, p|c, b, p|
|5|a, f, c, e, l, p, m, n|f, c, a, m, p|

Duyệt các Items phổ biến của mỗi giao dịch để xây dựng FP-Tree:

<hr>
<center><img src="http://i.imgur.com/bFdShL6.png"/></center>
<center>Hình 1. FP-Tree được xây dựng sau khi duyệt các items phổ biến của các giao dịch.</center>
<hr>

Tiếp theo, duyệt các item phổ biến mức 1 theo thứ tự tăng dần độ hỗ trợ là `p, m, b, a, c, f`. Với mỗi item, xây dựng các **cơ sở mẫu điều kiện** _(conditional pattern-base)_ và sau đó là các **FP-Tree điều kiện** _(conditional FP-Tree)_ của nó.

**Tính chất**: bất kì mẫu phổ biến nào có chứa mục I<sub>i</sub> đều được chứa trên các nhánh (đường dẫn) của cây FP-Tree chứa I<sub>i</sub>, số lần xuất hiện của mẫu chứa các nút trong đường dẫn tiền tố bằng số lần xuất hiện của nút I<sub>i</sub>.

Bắt đầu với item `p`, cơ sở mẫu điều kiện của nó là tất cả các đường dẫn tiền tố của cây FP-Tree khi duyệt từ gốc `root = null` đến nút `p`, chính là `fcam:2` và `cb:1` (số theo sau là số lần xuất hiện của nút `p` tương ứng với mỗi tiền tố đó).

Tiếp theo ta xây dựng FP-Tree điều kiện từ mẫu này bằng cách trộn tất cả các đường dẫn và giữ lại các nút có tổng các số đếm ≥ sup. = 3:

`fcam:2` và `cb:1` trộn lại thành `f:2, c:3, a:2, m:2, b:1`; chỉ có `c:3` là thoả mãn điều kiện.

Do đó, các mẫu phổ biến chứa p là: `p, cp`.

Làm tương tự cho các item còn lại, ta sẽ tìm được các mẫu phổ biến chứa các item đó. Cuối cùng có bảng sau đây:

|Item|Cơ sở mẫu điều kiện|FP-Tree điều kiện|Các mẫu phổ biến|
|-|-|-|-|
|p|{fcam:2, cb:1}|{c:3}-p|p, cp|
|m|{fca:2, fcab:1}|{f:3, c:3, a:3}-m|m, fm, cm, am, fcm, cam, fam, fcam|
|b|{fca:1, f:1, c:1}|∅|b|
|a|{fc:3}|{f:3, c:3}-a|a, fa, ca, fca|
|c|{f:3}|{f:3}-c|c, fc|
|f|∅|∅|f|

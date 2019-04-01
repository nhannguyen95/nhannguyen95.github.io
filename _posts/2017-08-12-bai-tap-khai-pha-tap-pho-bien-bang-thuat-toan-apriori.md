---
layout: post
title:  "Bài tập khai phá tập phổ biến bằng thuật toán Apriori"
date: 2017-08-12 16:46:00 +0700
tags:
  - data mining
no-post-nav: 0
comments: 1
---

##### **Bài toán**

Cho bảng dữ liệu bao gồm các giao dịch (tid) sau:

|TID|Items|
|-|-|
|100|1 3 4|
|200|2 3 5|
|300|1 2 3 5|
|400|2 5|

Tìm các tập mục có độ hỗ trợ ≥ 0.5 (tức tần số sup. ≥ 2).

##### **Lý thuyết**

###### **Các định nghĩa cơ bản**

**Tập hạng mục** _(Itemset)_

Giả sử `I` là một tập hữu hạn, mỗi phần tử của `I` gọi là một hạng mục (Item).

Một tập hạng mục `X` là một tập con của `I`.

`X` gọi là một tập hạng mục mức `k` _(k_itemset)_ nếu `X` chứa `k` hạng mục.

Ví dụ: `I = {1 2 3 4 5}`; `1, 2, 3, 4, 5` là các hạng mục; `X = {2 5}` là một tập hạng mục mức 2.

**Giao dịch** _(Transaction)_

Một tập các giao dịch xác định trên I là một ánh xạ `T:{1,2,..,n} → P(I)`. Tập `T(k)` là giao dịch thứ `k` của `T`.

Các số `1,..,n` là các định danh giao dịch (tids).

Ví dụ: giao dịch thứ `100`, `Tids(100) = {1 3 4}`.

**Ký hiệu tập các giao dịch chứa tập hạng mục**

Giả sử `X = {1 3}`, tập các định danh giao dịch có chứa `X` là `{100, 300}`, kí hiệu là `λ(X)`:

$$
\lambda(X) = \left \{ k \mid X \subseteq T(k) \right \}
$$

**Độ hỗ trợ** _(Support)_

Độ hỗ trợ của một item set `X` ký hiệu là `sup(X)` được xác định bởi công thức:

$$
sup(X) = \frac{\mid \lambda(X) \mid}{\mid T \mid}
$$

Ví dụ: `sup({1 3}) = 2/4 = 1/2`.

**Tập phổ biến/ tập thường xuyên** _(Large itemset/frequent itemset)_

Một itemset `X` được gọi là phổ biến hay tập thường xuyên nếu:

$$
sup(X) \geq \delta
$$

trong đó `𝛿`: minsup là một ngưỡng do người dùng xác định.

Ngược lại thì `X` được gọi là tập không phổ biến _(small itemset)_

**Luật kết hợp**

Một luật kết hợp là một công thức có dạng `X ⇒ Y`, trong đó `X`, `Y` là hai itemset thoả `X ∩ Y = ∅`, `X` được gọi là tiền đề và `Y` được gọi là hệ quả của luật.

Ví dụ: `2 ⇒ {3 5}`.

Luật kết hợp chỉ có ý nghĩa khi tần suất thể hiện mối tương quan giữa các tập thuộc tính là lớn hơn một ngưỡng nào đó.

**Độ hỗ trợ của luật kết hợp**

Độ hỗ trợ của một luật `X ⇒ Y`, ký hiệu `sup(X ⇒ Y)` là khả năng mà tập giao dịch `T` hỗ trợ cho các thuộc tính trong cả `X` và `Y`.

$$
sup(X \Rightarrow Y) = \frac{\mid \lambda(X \cup Y) \mid}{\mid T \mid}
$$

**Độ tin cậy của luật kết hợp**

Độ tin cậy của một luật `X ⇒ Y`, ký hiệu `conf(X ⇒ Y)` là xác suất có điều kiện $$ P(Y \mid X)$$:

$$
conf(X \Rightarrow Y) = \frac{\mid \lambda(X \cup Y)\mid}{\mid \lambda(X) \mid}
$$

Ví dụ: xét luật kết hợp `2 ⇒ {3 5}`, ta có `sup(2 ⇒ {3 5}) = 2/4 = 1/2`, `conf(2 ⇒ {3 5}) = 2/3`.

###### **Các tính chất**

**Các tính chất của large itemset**

Nếu $$ A \subseteq B$$ và $$ A, B$$ là các Itemset thì $$ sup(A) \geq sup(B)$$.

**Tính chất Apriori**
* Mọi tập con của một tập phổ biến đều phổ biến, nghĩa là:

$$
\forall Y \subseteq X, sup(X) \geq minsup \Rightarrow sup(Y) \geq minsup
$$

* Mọi tập mẹ của một tập không phổ biến đều không phổ biến, nghĩa là:

$$
\forall Y \supseteq X, sup(X) < minsup \Rightarrow sup(Y) < minsup
$$

##### **Đặt vấn đề**

Với một tập giao dịch `T`, mục đích của bài toán phát hiện luật kết hợp là tìm ra tất cả các luật có:
* độ hỗ trợ ≥ minsup và
* độ tin cậy ≥ minconf.

Thuật toán gồm hai bước:
* Sinh ra các tập mục phổ biến có độ hỗ trợ ≥ minsup.
* Sinh ra các luật kết hợp:
  * Từ mỗi tập mục phổ biến, sinh ra tất cả các luật có độ tin cậy cao (≥ minconf).
  * Mỗi luật là một phân tách nhị phân của một tập mục phổ biến.

Bước sinh ra các tập mục phổ biến có độ phức tạp cao, chúng ta tập trung vào bài tập tìm tập mục phổ biến (các tập có độ hỗ trợ ≥ minsup).

##### **Thuật toán Apriori**

Thuật toán do Agrawal đề nghị năm 1994, ý tưởng của thuật toán dựa vào tính chất Apriori: các ứng viên k+1_itemset có độ hỗ trợ ≥ μ, phải được sinh ra từ các k_itemset có độ hỗ trợ ≥ μ.

Xem lời giải dưới đây để hiểu cách triển khai thuật toán.

##### **Lời giải**

Đầu tiên tìm ra các itemset mức 1 và tính sup. của chúng dựa vào dữ liệu đã cho, đây gọi là tập ứng viên mức 1 - C<sub>1</sub>:

|itemset|sup.|
|-|-|
|{1}|2|
|{2}|3|
|{3}|3|
|{4}|1|
|{5}|3|

Loại những itemset có sup. < 2, tập những itemset này kí hiệu là L<sub>1</sub>:

|itemset|sup.|
|-|-|
|{1}|2|
|{2}|3|
|{3}|3|
|{5}|3|

Những itemset mức 2 có sup. ≥ 2 chỉ có thể sinh ra từ những itemset mức 1 có sup. ≥ 2 (tức những itemset mức 1 vừa liệt kê), liệt kê các itemset mức 2 có thể (C<sub>2</sub>) và tính sup. tương ứng:

|itemset|sup.|
|-|-|
|{1 2}|1|
|{1 3}|2|
|{1 5}|1|
|{2 3}|2|
|{2 5}|3|
|{3 5}|2|

Lặp lại, loại những itemset có sup. < 2, ta được L<sub>2</sub>

|itemset|sup.|
|-|-|
|{1 3}|2|
|{2 3}|2|
|{2 5}|3|
|{3 5}|2|

Tính tập ứng viên C<sub>3</sub>:
* Bước tổ hợp: {1 2 3}, {1 2 5}, {2 3 5}.
* Bước tỉa:
  * loại {1 2 3}, vì {1 2 3} sinh ra {1 2} không thuộc L<sub>2</sub>.
  * loại {1 2 5}, vì {1 2 5} sinh ra {1 2} không thuộc L<sub>2</sub>.
  * giữ {2 3 5}, vì các tập con của {2 3 5} là {2 3}, {2 5}, {3 5} đều thuộc L<sub>2</sub>.

Vậy tập ứng viên C<sub>3</sub> và sup. tương ứng:

|itemset|sup.|
|-|-|
|{2 3 5}|2|

Nhận thấy L<sub>4</sub> = C<sub>4</sub>.

Tập L<sub>4</sub> chỉ còn duy nhất 1 phần tử, thuật toán kết thúc.

Vậy các tập mục có độ hỗ trợ ≥ 0.5 bao gồm {1}, {2}, {3}, {5}, {1 3}, {2 3}, {2 5}, {3 5}, {2 3 5}.

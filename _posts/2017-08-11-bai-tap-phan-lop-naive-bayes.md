---
layout: post
title:  "Bài tập phân lớp sử dụng Naive Bayes"
date: 2017-08-11 23:40:00 +0700
categories: ['data mining']
tags:
no-post-nav: 0
comments: 1
---

##### **Bài toán**

Sử dụng Naive Bayes, phân lớp cho tập thuộc tính `(A1, A2) = (1, 1)`:

|A<sub>1</sub>|A<sub>2</sub>|Lớp|
|-|-|-|
|1|0|C<sub>1</sub>|
|0|0|C<sub>1</sub>|
|2|1|C<sub>2</sub>|
|1|2|C<sub>2</sub>|
|0|1|C<sub>1</sub>|
|1|1|?|

##### **Lý thuyết**

Cho C<sub>1</sub>, C<sub>2</sub>,..,C<sub>n</sub> là phân hoạch không gian mẫu M. A là biến cố được dự đoán thuộc phân hoạch (lớp) C<sub>i</sub> nếu:

$$ P(C_{i} | A) > P(C_{j}|A) ~~ \forall ~~ j\neq i, j = 1..n$$

Nghĩa là tiến hành so sánh các xác suất có điều kiện của biến cố C<sub>i</sub> được tính với điều kiện biến cố A đã xảy ra, và chọn xác suất lớn nhất.

Theo [định lý Bayes](https://en.wikipedia.org/wiki/Bayes%27_theorem), ta có:

$$ P(C_{i}|A) =\frac{P(A|C_{i})P(C_{i})}{P(A)} ~~~~~ (1)$$


Như vậy để so sánh $$ P(C_{i} \mid A)$$, ta so sánh phần tử của (1). Theo [xác suất kết hợp](https://en.wikipedia.org/wiki/Joint_probability_distribution), phần tử có thể biến đối thành:

$$ P(A|C_{i})P(C_{i}) = P(A, C_{i}) ~~ (=P(A \cap C_{i}))$$

Giả sử $$ A = \bigcap_{j=1}^{k}A_{j}$$, và các biến cố A<sub>j</sub> độc lập.

Suy ra:

$$ P(A, C_{i}) = P(A_{1},A_{2},..,A_{k},C_{i})$$

Theo [chain rule](https://en.wikipedia.org/wiki/Chain_rule_(probability)):

$$ P(A_{1},A_{2},..,A_{k},C_{i}) = P(C_{i})\prod_{i=1}^{k}P(A_{i}|A_{i+1},..,A_{k},C_{i}) ~~~~~ (2)$$

vì các A<sub>i</sub> độc lập nên:

$$ P(A_{i}|A_{i+1},..,A_{k},C_{i}) = P(A_{i}|C_{i})$$

Do đó $$ (2)$$ trở thành:

$$ P(A_{1},A_{2},..,A_{k},C_{i}) = P(C_{i})\prod_{i=1}^{k}P(A_{i}|C_{i})$$

Như vậy, thay vì so sánh các $$ P(C_{i} \mid A)$$ , chúng ta có thể so sánh các $$ P(C_{i})\prod_{i=1}^{k}P(A_{i} \mid C_{i})$$.

##### **Lời giải**

Quay lại bài toán trên, để phân lớp khi thuộc tính `X = (A1, A2) = (1, 1)`, ta so sánh 2 xác suất có điều kiện:

$$ P(C_{1} \mid X)$$

<center> và </center>

$$ P(C_{2} \mid X)$$


Theo lý thuyết, thay vì so sánh 2 xác suất trên, ta đi so sánh:

$$ P(C_{1})P(A_{1}=1 \mid C_{1})P(A_{2}=1 \mid C_{1})$$

<center>và</center>

$$ P(C_{2})P(A_{1}=1 \mid C_{2})P(A_{2}=1 \mid C_{1})$$

Lần lượt có:

$$ P(C_{1}) = \frac{3}{5}$$, $$ P(C_{2}) = \frac{2}{5}$$

$$ P(A_{1}=1 \mid C_{1}) = \frac{1}{3}$$, $$ P(A_{2}=1 \mid C_{1}) = \frac{1}{3}$$

$$ P(A_{1}=1 \mid C_{2}) = \frac{1}{2}$$, $$ P(A_{2}=1 \mid C_{2}) = \frac{1}{2}$$

Sau khi tính toán, nhận thấy rằng:

$$ P(C_{1} \mid X) < P(C_{2} \mid X) ~~~~~ (\frac{1}{15}<\frac{1}{10})$$

Vậy `X = (A1, A2) = (1, 1)` thuộc lớp C<sub>2</sub>.

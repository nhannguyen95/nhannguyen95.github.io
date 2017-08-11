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
|1|0|1|
|0|0|1|
|2|1|2|
|1|2|2|
|0|1|1|
|1|1|?|

##### **Lý thuyết**

Cho H<sub>1</sub>, H<sub>2</sub>,..,H<sub>n</sub> là phân hoạch không gian mẫu M. A là biến cố được dự đoán thuộc phân hoạch (lớp) H<sub>i</sub> nếu:

$$
P(H_{i} | A) > P(H_{j}|A) ~~ \forall ~~ j\neq i, j = 1..n
$$

Nghĩa là tiến hành so sánh các xác suất có điều kiện của biến cố H<sub>i</sub> được tính với điều kiện biến cố A đã xảy ra, và chọn xác suất lớn nhất.

Theo [định lý Bayes](https://en.wikipedia.org/wiki/Bayes%27_theorem), ta có:

$$
P(H_{i}|A) =\frac{P(A|H_{i})P(H_{i})}{P(A)} ~~~~~ (1)
$$

Như vậy để so sánh $$ P(H_{i}|A)$$, ta so sánh phần tử của $$ (1)$$. Theo [xác suất kết hợp](https://en.wikipedia.org/wiki/Joint_probability_distribution), phần tử có thể biến đối thảnh:

$$
P(A|H_{i})P(H_{i}) = P(A, H_{i}) ~~ (=P(A \cap H_{i}))
$$

Giả sử:

$$
A = \bigcap_{j=1}^{k}A_{j}
$$

và các biến cố A<sub>j</sub> độc lập.

Suy ra:

$$
P(A, H_{i}) = P(A_{1},A_{2},..,A_{k},H_{i})
$$

Theo [chain rule](https://en.wikipedia.org/wiki/Chain_rule_(probability)):

$$
P(A_{1},A_{2},..,A_{k},H_{i}) = P(H_{i})\prod_{i=1}^{k}P(A_{i}|A_{i+1},..,A_{k},H_{i}) ~~~~~ (2)
$$

vì các A<sub>i</sub> độc lập nên:

$$
P(A_{i}|A_{i+1},..,A_{k},H_{i}) = P(A_{i}|H_{i})
$$

Do đó $$ (2)$$ trở thành:

$$
P(A_{1},A_{2},..,A_{k},H_{i}) = P(H_{i})\prod_{i=1}^{k}P(A_{i}|H_{i})
$$

Như vậy, thay vì so sánh $$ P(H_{i}|A)$$, chúng ta có thể so sánh $$ P(H_{i})\prod_{i=1}^{k}P(A_{i}|H_{i})$$.

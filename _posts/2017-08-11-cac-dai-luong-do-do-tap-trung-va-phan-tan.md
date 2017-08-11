---
layout: post
title:  "Các đại lượng số để đo độ tập trung và độ phân tán của dữ liệu"
date: 2017-08-11 15:46:00 +0700
categories: ['statistics']
tags:
no-post-nav: 0
comments: 1
---

##### **Introduction**
Xác định các đại lượng số (đại lượng thống kê mô tả) để đo độ tập trung (**central tendency**) và độ phân tán (**dispersion**) của dữ liệu.

Các đại lượng này kết hợp với đồ thị phân phối tần số sẽ cho một bức tranh rõ nét chi tiết về tập dữ liệu cần xử lý.

##### **Các đại lượng đo độ tập trung**

###### **Trung bình cộng đơn giản (Mean)**:

$$
\bar{x}=\frac{\sum_{i=1}^{n}x_{i}}{n}
$$

trong đó:
* n là cỡ mẫu.
* x<sub>i</sub> là giá trị quan sát thứ i.

###### **Trung bình cộng có trọng số (Weighted mean)**:

$$
\bar{X}_{w}=\frac{\sum_{i=1}^{n}w_{i}x_{i}}{\sum_{i=1}^{n}w_{i}}
$$

trong đó:
* {x<sub>1</sub>, x<sub>2</sub>,..,x<sub>n</sub>}: tập dữ liệu mẫu.
* {w<sub>1</sub>, w<sub>2</sub>,..,w<sub>n</sub>}: tập trọng số tương ứng với tập dữ liệu mẫu.

###### **Trung vị (Meadian)**:

$$
Me=
\begin{cases}
x_{[(n+1)/2]} & \text{ if n odd}  \\
(x_{[n/2]} + x_{[(n+2)/2]}) / 2 & \text{ if n even}
\end{cases}
$$

với tập dữ liệu mẫu {x<sub>1</sub>, x<sub>2</sub>,..,x<sub>n</sub>} đã được sắp xếp.

###### **Yếu vị (Mode)**:

Là giá trị được gặp nhiều lần trong tập dữ liệu mẫu.

###### **Trung bình nhân (Geometri mean)**:

$$
\bar{x}=\sqrt[n]{x_{1}x_{2}..x_{n}}
$$

###### **Giá trị trung bình của giá trị lớn nhất và nhỏ nhất trong tập dữ liệu (Midrange)**:

$$
\frac{x_{max}+x_{min}}{2}
$$

##### **Các đại lượng đo độ phân tán**

###### **Phân vị (Percentiles)**:

Công thức xác định phân vị thứ p:

$$
i = \frac{p}{100}(n+1)
$$

**Phân vị thứ p** (0 < p < 100) trong một dãy tăng dần gồm n phần tử là một giá trị chia dãy số thành 2 phần, một phần gồm p% số quan sát có giá trị nhỏ hơn hoặc bằng giá trị phân vị thứ p.

###### **Khoảng biến thiên (Range)**:

$$
R = x_{max} - x_{min}
$$

###### **Độ trãi giữa (Interquartile Range)**:

$$
IRQ = Q_{3} - Q_{1}
$$

trong đó:
* Q<sub>1</sub> là phần vị thứ 25
* Q<sub>3</sub> là phần vị thứ 75

###### **Phương sai (Variance)**:

$$
s^{2}=\frac{\sum_{i=1}^{n}(X_{i} - \bar{X})^{2}}{n-1}
$$

trong đó:
* x̄ là trung bình cộng đơn giản.

###### **Độ lệch chuẩn (Standard deviation)**:

$$
s=\sqrt{\frac{\sum_{i=1}^{n}(X_{i} - \bar{X})^{2}}{n-1}}
$$

trong đó:
* x̄ là trung bình cộng đơn giản.

###### **Quan sát ngoại lệ (Outliers)**:

Các giá trị thường nằm cách trên Q<sub>3</sub> hay dưới Q<sub>1</sub> một khoảng 1.5IRQ.

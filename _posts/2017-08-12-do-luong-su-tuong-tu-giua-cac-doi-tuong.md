---
layout: post
title:  "Đo lường sự tương tự giữa các đối tượng"
date: 2017-08-12 14:05:00 +0700
categories: ['data mining']
tags:
no-post-nav: 0
comments: 1
---

##### **Introduction**

Đo lường sự tương tự _(similarity measure)_ cho biết hai đối tượng giống nhau đến mức nào. Đo lường sự tương tự trong khai phá dữ liệu _(data mining)_ là khoảng cách giữa các chiều được biểu diễn bằng các đặc trưng của các đối tượng. Khoảng cách càng nhỏ, hai đối tượng càng tương tự nhau.

Đo lường sự tương tự giữa các đối tượng là rất quan trọng và là cơ sở để triển khai các kỹ thuật:
* Nhận dạng _(reconize)_
* Phân lớp _(classification)_
* Phân cụm _(clustering)_
* Dự đoán _(prediction)_

Dưới đây là những độ đo khoảng cách thông dụng cho các loại dữ liệu khác nhau:

* Kiểu nguyên, khoảng _(integer, interval)_
* Kiểu nhị phân _(binary)_
* Kiểu định danh _(nominal)_

##### **Khoảng cách cho kiểu nguyên, khoảng**

###### **Khoảng cách Euclide**

$$ d(x_{i},x_{j})=\sqrt{\sum_{k=1}^{m}(x_{ik}-x_{jk})^{2}}$$

###### **Khoảng cách Manhattan**

$$ d(x_{i},x_{j})=\sum_{k=1}^{m}\mid x_{ik}-x_{jk}\mid$$

###### **Khoảng cách Minkowski**

$$ d(x_{i},x_{j})=(\sum_{k=1}^{m}\mid x_{ik}-x_{jk}\mid^{p})^{\frac{1}{p}}$$

###### **Khoảng cách có trọng số**

$$ d(x_{i},x_{j})=(\sum_{k=1}^{m}w_{k}\mid x_{ik}-x_{jk}\mid^{p})^{\frac{1}{p}}$$

##### **Khoảng cách cho kiểu nhị phân**

Gọi:
* p là số biến có giá trị positive của cả 2 đối tượng.
* q là số biến có giá trị positive của đối tượng thứ i và negative của đối tượng thứ j.
* r là số biến có giá trị negative của đối tượng thứ i và positive của đối tượng thứ i.
* s là số biến có giá trị negative của cả 2 đối tượng.
* t = p + q + r + s là tổng số biến.

###### **Simple Matching Distance**

$$ d_{ij}=\frac{q+r}{t}$$

###### **Jaccard's Distance**

Sự khác nhau của hai đối tượng dựa trên các biến nhị phân bất đối xứng _(asymmetric binary dissimilarity)_

$$ d_{ij}=\frac{q+r}{p+q+r}$$

###### **Hamming Distance**

$$ d_{ij}=q + r$$

###### **Simple Matching Coefficient**

$$ d_{ij}=\frac{p+s}{t}$$

###### **Jaccard's Coefficient**

Đo khoảng cách giữa hai đối tượng dựa trên khái niệm tương tự (similarity) thay vì khác nhau (dissimilarity)

$$ d_{ij}=\frac{p}{p+q+r}$$

###### **Ví dụ**

Với x<sub>i</sub> = (1,1,1,1) và x<sub>j</sub> = (0,1,0,0), ta có:
* p = 1 (đặc trưng thứ hai)
* q = 3 (3 đặc trưng còn lại)
* r = 0
* s = 0
* t = 4

##### **Khoảng cách cho kiểu định danh**

Trong nhiều trường hợp, không thể đo lường các biến theo cách định lượng mà chúng có thể được đo lường theo cách phân loại hay định đanh. Đặc trưng chính của biến định danh là được gán nhãn (labeling) và không quan tâm đến thứ tự.

Để tính khoảng cách giữa 2 đối tượng được biểu diễn bởi các biến định danh (hay phân loại), cần quan tâm đến số giá trị phân loại trong mỗi biến:
* Nếu chỉ có 2 giá trị phân loại, ta có thể dùng cách tính khoảng cách cho biến nhị phân.
* Nếu số giá trị phân loại lớn hơn 2, chúng ta cần chuyển đổi những giá trị phân loại này sang tập các biến giả _(dummy variable)_ có giá trị nhị phân.

Giả sử một đối tượng được biểu diễn bởi hai biến là `Gender` và `Mode` (phương tiện đi lại). Biến `Gender` là biến nhị phân có 2 giá trị 0 = male và 1 = female, biến `Mode` gồm 3 giá trị là `Bus`, `Train` và `Van`. Ta chuyển mỗi giá trị của biến `Mode` sang biến giả nhị phân, ví dụ chọn `Mode = Train` thì biến giả nhị phân là `(0,1,0)`.

Giả sử có 3 đối tượng:
* `Alex` (`Male`) sử dụng `Bus`.
* `Brian` (`Male`) sử dụng `Van`.
* `Cherry` (`Female`) sử dụng `Bus`.

3 đối tượng này được biểu diễn như sau:
* `Alex = (0, (1,0,0))`
* `Brian = (0, (0,0,1))`
* `Cherry = (1, (1,0,0))`

Để tính khoảng cách giữa các đối tượng, ta cần tính khoảng cách cho mỗi biến ban đầu.

Giả sử sử dụng Hamming distance:
* `Distance(Alex, Brian) = (0, 2)`, tổng khoảng cách là `0 + 2 = 2`.
* `Distance(Alex, Cherry) = (1, 0)`, tổng khoảng cách là `1 + 0 = 1`.
* `Distance(Brian, CherryCherry) = (1, 2)`, tổng khoảng cách là `1 + 2 = 3`.

Khoảng cách cũng có thể là tỉ lệ giữa số đặc trưng khác nhau và tổng số biến giả:
* `Distance(Alex, Brian) = (0, 2/3)`, tổng khoảng cách là `(0 + 2/3) / 2 = 1/3`.
* `Distance(Alex, Cherry) = (1, 0)`, tổng khoảng cách là `(1 + 0) / 2 = 1/2`.
* `Distance(Brian, CherryCherry) = (1, 2/3)`, tổng khoảng cách là `(1 + 2/3) / 2 = 5/6`.

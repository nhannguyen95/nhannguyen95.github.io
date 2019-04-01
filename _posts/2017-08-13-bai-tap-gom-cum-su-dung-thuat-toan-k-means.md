---
layout: post
title:  "Bài tập gom cụm sử dụng thuật toán K-Means"
date: 2017-08-13 16:09:00 +0700
tags:
  - data mining
no-post-nav: 0
comments: 1
---

##### **Bài toán**

Giả sử có 4 sinh viên A, B, C, D. Mỗi sinh viên được biểu diễn bởi hai đặc trưng X, Y.

|Sinh viên|Sở thích(X)|Quê quán(Y)|
|-|-|-|
|A|1|1|
|B|2|1|
|C|4|3|
|D|5|4|

Mục đích là nhóm các sinh viên đã cho vào 2 nhóm (K = 2) dựa vào các đặc trưng của chúng.

##### **Thuật toán gom cụm K-Means**

**Đầu vào của thuật toán**: số cụm k, và CSDL có n đối tượng.

**Thuật toán gồm 4 bước:**
* Bước 1: Chọn ngẫu nhiên k điểm làm trọng tâm ban đầu của k cụm.

* Bước 2: Gán (hoặc gán lại) từng điểm vào cụm có trọng tâm gần điểm đang xét nhất.
  * Nếu không có phép gán lại nào thì dừng.
    * Vì không có phép gán lại nào có nghĩa là các cụm đã ổn định và thuật toán không thể cải thiện làm giảm độ phân biệt hơn được nữa.


* Bước 3: Tính lại trọng tâm cho từng cụm.

* Bước 4: Quay về bước 2.

##### **Lời giải**

Bước 1: Chọn tâm cho 2 nhóm. Giả sử chọn A làm tâm nhóm thứ nhất (toạ độ tâm nhóm thứ nhất c<sub>1</sub>(1, 1)) và B là tâm nhóm thứ hai (toạ độ tâm nhóm thứ hai c<sub>2</sub>(2, 1)).

Bước 2: Tính khoảng cách từ các đối tượng đến tâm của các nhóm (khoảng cách Euclide):

$$
D^{0}=\begin{bmatrix}
0 & 1 & 3.61 & 5\\  
1 & 0 & 2.83 & 4.24  
\end{bmatrix}
$$

_Nếu đánh số A, B, C, là 1, 2, 3, 4; thì D<sup>0</sup>(i, j) là khoảng cách của điểm j đến tâm c<sub>i</sub>_.

Bước 3: Nhóm các đối tượng vào nhóm gần nhất

$$
G^{0}=\begin{bmatrix}
1 & 0 & 0 & 0\\  
0 & 1 & 1 & 1
\end{bmatrix}
$$

_G<sup>0</sup>(i, j) = 1 nghĩa là điểm j thuộc nhóm i_.

Bước 4: Tính lại toạ độ tâm cho các nhóm mới dựa vào toạ độ của các đối tượng trong nhóm. Nhóm 1 chỉ có đối tượng A nên tâm nhóm 1 vẫn không đổi, c<sub>1</sub>(1, 1). Tâm nhóm 2 được tính như sau:

$$
c_{2} = (\frac{2+4+5}{3},\frac{1+3+4}{3} = (\frac{11}{3}, \frac{8}{3})
$$

Bước 5: Tính lại khoảng cách từ các đối tượng đến tâm mới:

$$
D^{1}=\begin{bmatrix}
0 & 1 & 3.61 & 5\\  
3.14 & 2.36 & 0.47 & 1.89
\end{bmatrix}
$$

Bước 6: Nhóm các đối tượng vào nhóm:

$$
G^{1}=\begin{bmatrix}
1 & 1 & 0 & 0\\  
0 & 0 & 1 & 1
\end{bmatrix}
$$

Bước 7: Tính lại tâm cho nhóm mới:

$$
c_{1}=(\frac{1+2}{2}, \frac{1+1}{2}) = (1.5,1)
$$

$$
c_{2}=(\frac{4+5}{2}, \frac{3+4}{2}) = (4.5,3.5)
$$

Bước 8: Tính lại khoảng cách từ các đối tượng đến tâm mới:

$$
D^{2}=\begin{bmatrix}
0.5 & 0.5 & 3.2 & 4.61\\  
4.3 & 3.54 & 0.71 & 0.71
\end{bmatrix}
$$

Bước 9: Nhóm các đối tượng vào nhóm:

$$
G^{2}=\begin{bmatrix}
1 & 1 & 0 & 0\\  
0 & 0 & 1 & 1
\end{bmatrix}
$$

Ta có $$ G^{2}=G^{1}$$ nên thuật toán dừng. Vậy kết quả phân nhóm: A, B nhóm 1; C, D nhóm 2.

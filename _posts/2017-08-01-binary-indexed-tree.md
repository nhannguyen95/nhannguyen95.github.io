---
layout: post
title:  "Binary Indexed Tree (BIT)"
date: 2017-08-01 18:08:00 +0700
categories:
tags: bit
no-post-nav: 0
comments: 1
---

##### **Đề bài**
Cho dãy số nguyên a<sub>i</sub> gồm N phần tử và Q truy vấn thuộc một trong hai dạng sau:
* 1 x y: thay đổi a[x] += y.
* 2 x y: cho biết tổng a[x]+a[x+1]+..+a[y]

**Giới hạn:**

* 1 ≤ N ≤ 10<sup>5</sup>
* 1 ≤ Q ≤ 10<sup>5</sup>

##### **Định dạng test**
**Input:**

* Dòng đầu tiên là N và Q.
* Dòng tiếp theo là N số nguyên a<sub>i</sub>.
* Q dòng tiếp theo là Q truy vấn thuộc một trong hai dạng trên.

**Output:**
* Các dòng trả lời cho các truy vấn loại hai.

Ví dụ:

{% highlight text %}
input:
5 5
2 4 8 2 1
2 1 5
1 2 10
2 1 3
1 3 -3
2 3 4
---
output:
17
24
7
{% endhighlight %}

##### **Thuật toán**

Có thể sử dụng Binary Indexed Tree (BIT). Tài liệu tham khảo:

[Hackerearth](https://www.hackerearth.com/practice/notes/binary-indexed-tree-or-fenwick-tree/)

[Topcoder](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-indexed-trees/)

Cuối cùng, một bức hình hơn vạn lời nói:

<hr>
<center><img src="http://i.imgur.com/l8kIbAo.jpg" width="400"/></center>
<center>Hình 1. BIT for a 17 element array.</center>
<center><i><a href="https://www.hackerearth.com/practice/notes/binary-indexed-tree-or-fenwick-tree/">Source: Hackerearth BIT tutorial</a></i></center>
<hr>

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1e5 + 5;
int n, q, a[N], BIT[N];

void update(int i, int v) {
    for(; i <= n; i += (i&-i))
        BIT[i] += v;
}

// prefix sum i: a[1] + .. + a[i]
int query(int i) {
    int sum = 0;
    for(; i >= 1; i -= (i&-i))
        sum += BIT[i];
    return sum;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input & init
    cin >> n >> q;
    for(int i = 1; i <= n; i++) {
        cin >> a[i];
        update(i, a[i]);
    }

    // solve query
    while(q--) {
        int t, x, y;
        cin >> t >> x >> y;

        if (t == 1) update(x, y);
        else cout << query(y) - query(x-1) << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
Hàm query, update: O(logN).

Độ phức tạp tổng quát của bài toán: O(NlogN + QlogN).

---
layout: post
title:  "Đếm số cặp (a, b) sao cho a & b = n"
date: 2017-07-30 14:34:00 +0700
categories:
tags:
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codeforces 677C](http://codeforces.com/problemset/problem/677/C)

##### **Đề bài**
Cho số nguyên dương n, đếm số cặp số (a, b) sao cho a & b = n.

**Giới hạn:**

* 0 ≤ a,b,n ≤ 2<sup>k</sub>
* 1 ≤ k ≤ 30

##### **Định dạng test**
**Input:**

* Chứa duy nhất một dòng là n.

**Output:**
* In ra số cặp số (a, b): a & b = n chia lấy dư cho 10<sup>9</sup> + 7.

**Ví dụ:**

{% highlight text %}
input:
5 3
output:
3
{% endhighlight %}

Có 3 cặp số (a, b) thoả mãn là: (5, 5), (7, 5), (5, 7).

##### **Thuật toán**

Duyệt qua k bit của n, tại bit thứ i:
* Nếu bit = 0, có 3 khả năng để có kết quả là 0: 0&0, 1&0, 0&1. Nghĩa là có 3 khả năng cho bit thứ i của số a và b, nhân 3 vào kết quả.
* Nếu bit = 1, có 1 khả năng để có kết quả là 1: 1 & 1. Tương tự, nhân 1 vào kết quả.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const ll MOD = 1e9 + 7;

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input
    int n, k;
    cin >> n >> k;

    //solve
    ll res = 1;
    for(int i = 0; i < k; i++)
        if ((n&(1<<i)) == 0) (res *= 3) %= MOD;

    // ouput
    cout << res;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(k)

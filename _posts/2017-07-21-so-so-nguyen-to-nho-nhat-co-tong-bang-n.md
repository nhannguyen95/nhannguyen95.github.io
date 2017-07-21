---
layout: post
title:  "Số số nguyên tố nhỏ nhất có tổng bằng N"
date: 2017-07-21 14:34:00 +0700
categories:
tags: math
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codeforces 735D](http://codeforces.com/problemset/problem/735/D)

##### **Đề bài**
Cho số nguyên N > 0, tìm số số nguyên tố nhỏ nhất có tổng bằng N.

**Giới hạn:**

* 2 ≤ n ≤ 2·10<sup>9</sup>

##### **Định dạng test**
**Input:**

* Một dòng duy nhất là số N.

**Output:**
* Số số nguyên tố nhỏ nhất có tổng bằng N.

Ví dụ:

{% highlight text %}
27
---
3
{% endhighlight %}

*(27 = 2 + 2 + 23)*

##### **Lời giải**

Với N = 1, bài toán không có lời giải.

Nếu N là số nguyên tố, cách phân tích tối ưu là N = N, đáp số là 1.

Nếu N chẵn và N > 2, theo [giả thuyết Goldbach](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture): có thể biểu diễn N thành tổng của hai số nguyên tố, điều này được chứng minh đúng với N ≤ 4·10<sup>18</sup>.

Nếu N lẻ:
* Các số nguyên tố > 2 là số lẻ, tổng của các số lẻ là số chẵn => trong cách phân tích N thành tổng các số nguyên tố, phải có một số nguyên tố chẵn, mà 2 là số nguyên tố chẵn duy nhất => N = 2 + (N-2). Đến đây, nếu (N-2) là số nguyên tố, thì đáp số là 2. Ngược lại:
* Phân tích N = 3 + (N-3), N-3 là một số chẵn, lại theo [giả thuyết Goldbach](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture), N-3 là tổng của 2 số nguyên tố => kết quả là 3.

##### **Thuật toán**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
bool isPrime(int x) {
    if (x == 1) return false;
    if (x == 2 || x == 3) return true;
    if (x % 2 == 0 || x % 3 == 0) return false;
    for(int i = 3; i <= sqrt(x); i += 2)
        if (x % i == 0) return false;
    return true;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    int n; cin >> n;

    if (isPrime(n)) cout << 1;
    else if (n % 2 == 0) cout << 2;
    else if (isPrime(n-2)) cout << 2;
    else cout << 3;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(√n)

---
layout: post
title:  "Phân tích một số lẻ thành tổng của tối đa 3 số nguyên tố"
date: 2017-07-30 20:21:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codeforces 584D](http://codeforces.com/problemset/problem/584/D)

##### **Đề bài**
Cho số nguyên dương lẻ N, tìm cách phân tích N thành tổng của tối đa 3 số nguyên tố.

**Giới hạn:**

* 3 ≤ n ≤ 10<sup>9</sup>

##### **Định dạng test**
**Input:**

* Một dòng duy nhất là số N.

**Output:**
* Dòng đầu là số số nguyên tố có tổng bằng N.
* Dòng thứ hai là giá trị các số nguyên tố đó.

Ví dụ:

{% highlight text %}
27
---
3
5 11 11
{% endhighlight %}


##### **Thuật toán**

Có một tính chất thú vị và quan trọng: **khoảng cách giữa hai số nguyên tố liêp tiếp nhau không quá lớn. Với n = 10<sup>9</sup>, khoảng cách tối đa là 282.**

Như vậy, có thể tìm số nguyên tố p sao cho: p lớn nhất và p ≤ N; khi đó N - p <= 282, và là một số chẵn.

Bây giờ chúng ta cần phân tích số chẵn (N - p) thành tổng của tối đa 2 số nguyên tố. Luôn luôn làm được điều này dựa vào [giả thuyết Goldbach](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture) ([xem thêm](https://nhannguyen95.github.io/2017/07/21/so-so-nguyen-to-nho-nhat-co-tong-bang-n)): sử dụng brute force, tìm số nguyên tố P sao cho (N - p - P) cũng là số nguyên tố. Như vậy đáp số sẽ là 3 số: p, P và N - p - P.

*Việc đề cập đến các corner cases được bỏ qua.*

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
bool isPrime(int x) {
    if (x == 0 || x == 1) return false;
    if (x == 2 || x == 3) return true;
    if (x % 2 == 0 || x % 3 == 0) return false;
    for(int i = 3; i <= sqrt(x); i += 2)
        if (x % i == 0) return false;
    return true;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // init
    int n; cin >> n;
    vector<int> res;

    // find maximum prime p: p <= n;
    int p = n;
    while(!isPrime(p)) p--;
    res.pb(p);
    n -= p;

    // now 0 <= n <= 300 (and even), we can brute force
    if (isPrime(n)) res.pb(n);
    else {
        FOR(i,2,n)
        if (isPrime(i) && isPrime(n - i)) {
            res.pb(i);
            res.pb(n - i);
            break;
        }
    }

    // output
    cout << sz(res) << '\n';
    REP(i,sz(res)) cout << res[i] << ' ';

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
~ O(300√N)

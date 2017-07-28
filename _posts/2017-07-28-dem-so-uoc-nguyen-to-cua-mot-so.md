---
layout: post
title:  "Đếm số ước nguyên tố của một số"
date: 2017-07-28 15:23:00 +0700
categories:
tags: eratosthenes
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codeforces 546D](http://codeforces.com/problemset/problem/546/D)

##### **Đề bài**
Cho q truy vấn là q số nguyên n, cho biết số ước nguyên tố của n.

**Giới hạn:**

* 1 ≤ q ≤ 1e6
* 1 ≤ n ≤ 1e6

##### **Định dạng test**
**Input:**

* Dòng đầu là q.
* q dòng tiếp theo, mỗi dòng là một số n.

**Output:**
* q dòng trả lời cho q truy vấn: số ước nguyên tố của n.

**Ví dụ:**

{% highlight text %}
input:
5
1
11
45
10000
output:
0
1
3
8
{% endhighlight %}

##### **Thuật toán**

Định dạng [sàng Eratosthenes](https://vi.wikipedia.org/wiki/S%C3%A0ng_Eratosthenes): gọi primeFactor[n] là số ước nguyên tố của n, với mỗi số nguyên tố a, tính primeFactor[] cho các bội b của a như sau:

primeFactor[b] = primeFactor[b/a] + 1

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const ll N = 1e6 + 5;
int q, primeFactor[N];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    for(ll i = 2; i < N; i++)
        if (primeFactor[i] == 0)
            for(ll j = i; j < N; j += i)
                primeFactor[j] = primeFactor[j/i] + 1;

    cin >> q;
    while(q--) {
        int n; cin >> n;
        cout << primeFactor[n] << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
Xấp xỉ O(NlogNlogN)

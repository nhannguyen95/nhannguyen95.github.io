---
layout: post
title:  "Tổng các ước của một số"
date: 2017-08-01 16:53:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ NKABD](http://vn.spoj.com/problems/NKABD/)

##### **Đề bài**
Cho Q truy vấn là Q số N, với mỗi truy vấn cho biết tổng các ước của N (bao gồm cả 1 và chính N, nhưng điều này không quan trọng).

**Giới hạn:**

* 1 ≤ N ≤ 10<sup>6</sup>
* 1 ≤ Q ≤ 10<sup>6</sup>

##### **Định dạng test**
**Input:**

* Dòng đầu tiên là Q.
* Q dòng tiếp theo là Q số N.

**Output:**
* Q dòng là Q đáp án cho mỗi truy vấn.

Ví dụ:

{% highlight text %}
abbaabba
5
1
2
3
4
5
---
1
3
4
7
6
{% endhighlight %}

##### **Thuật toán**

Gọi dp[n] là tổng các ước của n. Với mỗi số j là bội của i, ta cập nhập dp[j] = dp[j] + i.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
ll dp[1000000 + 5];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // dp
    for(int i = 1; i <= 1e6; i++)
        for(int j = i; j <= 1e6; j+=i)
            dp[j] += i;

    // solve
    int q; cin >> q;
    while(q--) {
        int n; cin >> n;
        cout << dp[n] << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(NlogN + Q)

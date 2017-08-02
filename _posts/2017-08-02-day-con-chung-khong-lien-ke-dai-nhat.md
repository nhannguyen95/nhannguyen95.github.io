---
layout: post
title:  "Dãy con dài nhất có tổng lớn hơn bằng p"
date: 2017-08-02 22:57:00 +0700
categories:
tags: dp
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ LNACS](http://vn.spoj.com/problems/LNACS/)

##### **Đề bài**
Một dãy con không liền kề của một dãy số A là một dãy các phần tử của A không liền kề nhau.

Cho hai dãy số A và B có độ dài m và n, tìm độ dài của dãy số C là dãy con chung không liền kề dài nhất của A và B.

**Giới hạn:**

* 2 ≤ n, m ≤ 1000
* 0 < A<sub>i</sub>, B<sub>i</sub> ≤ 10000

##### **Định dạng test**
**Input:**

* Dòng đầu là m và n.
* Dòng tiếp theo là m phần tử của dãy A.
* Dòng tiếp theo là n phần tử của dãy B.

**Output:**
* Một số duy nhất là độ dài của dãy con chung không liền kề dài nhất của A và B.

Ví dụ:

{% highlight text %}
input:
4 5
4 9 2 4
1 9 7 3 4
---
output:
2
{% endhighlight %}

##### **Thuật toán**

Gọi dp[i][j] là độ dài dãy con chung không liền kề dài nhất khi xét đến phần tử thứ i của dãy A và phần tử thứ j của dãy B.

Công thức truy hồi:
* Nếu a[i] = b[j], dp[i][j] = 2 + dp[i-2][j-2].
* Ngược lại: dp[i][j] = max(dp[i-1][j], dp[i][j-1]).


##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1000 + 5;
int m, n, a[N], b[N], dp[N][N];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input
    cin >> m >> n;
    FOR(i,1,m) cin >> a[i];
    FOR(i,1,n) cin >> b[i];

    // dp init
    dp[0][0] = 0;

    // dp
    FOR(i,1,m)
        FOR(j,1,n) {

            dp[i][j] = max(dp[i-1][j], dp[i][j-1]);

            if (a[i] == b[j])
                dp[i][j] = max(dp[i][j], 1 + dp[max(0,i-2)][max(0,j-2)]);
        }

    cout << dp[m][n];

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N<sup>2</sup>)

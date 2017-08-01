---
layout: post
title:  "Truy vấn tổng các phần tử của hình chữ nhật con"
date: 2017-08-01 20:07:00 +0700
categories:
tags: dp
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ BONUS](http://vn.spoj.com/problems/BONUS/)

##### **Đề bài**
Cho một hình chữ nhật n hàng và m cột, hàng được đánh số từ 1 đến n từ trên xuống dưới, cột được đánh số từ 1 đến m từ trái qua phải. Kí hiệu (i, j) là ô tạo bởi hàng i và cột j. Tại mỗi ô của hình chữ nhật có một số nguyên a<sub>ij</sub>.

Cho Q truy vấn, mỗi truy vấn là một hình chữ nhật con của hình chữ nhật đã cho, với ô trên trái là (x1, y1) và ô dưới phải là (x2, y2). Cho biết tổng các phần tử trong hình chữ nhật con này trong O(1).

**Giới hạn:**

* 1 ≤ n ≤ 1000
* 1 ≤ a<sub>ij</sub> ≤ 1000
* 1 ≤ Q ≤ 10<sup>5</sup>
* 1 ≤ x1 ≤ x2 ≤ n, 1 ≤ y1 ≤ y2 ≤ m

##### **Định dạng test**
**Input:**

* Dòng đầu là N và M.
* N dòng tiếp theo, mỗi dòng là M số nguyên a<sub>ij</sub>.
* Dòng tiếp theo là Q.
* Q dòng tiếp theo là 4 số nguyên x1, y1, x2, y2.

**Output:**
* Q dòng trả lời cho Q truy vấn.

Ví dụ:

{% highlight text %}
input:
4 4
1 9 1 1
9 9 9 9
1 9 9 9
1 9 9 14
3
1 1 4 4
1 2 3 4
2 2 3 3
---
output:
109
65
36
{% endhighlight %}

##### **Thuật toán**

Tính dp[x][y] là tổng các phần tử của hình chữ nhật con có góc trái trên là (1, 1) và góc dưới phải là (x, y).

Như vậy tổng các phần tử của hình chữ nhật con tạo bởi (x1,y1) và (x2,y2) cho bởi công thức:

dp[x2][y2] - dp[x1-1][y2] - dp[x2][y1-1] + dp[x1-1][y1-1]

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1005;
int n, m, q, a[N][N], dp[N][N];

int getSum(int x1, int y1, int x2, int y2) {
    return dp[x2][y2]
    - dp[x1-1][y2]
    - dp[x2][y1-1]
    + dp[x1-1][y1-1];
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input
    cin >> n >> m;
    FOR(i,1,n) FOR(j,1,m) cin >> a[i][j];

    // dp
    FOR(i,1,n) {
        int row = 0;
        FOR(j,1,m) {
            row += a[i][j];
            dp[i][j] = dp[i-1][j] + row;
        }
    }

    // solve queries
    cin >> q;
    while(q--) {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;

        cout << getSum(x1, y1, x2, y2) << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N<sup>2</sup> + Q)

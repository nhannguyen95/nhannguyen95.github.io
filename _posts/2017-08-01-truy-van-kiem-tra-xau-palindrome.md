---
layout: post
title:  "Truy vấn kiểm tra xâu palindrome"
date: 2017-08-01 13:19:00 +0700
categories:
tags: palindrome dp
no-post-nav: 0
comments: 1
---

##### **Đề bài**
Cho xâu s có độ dài n và q truy vấn, mỗi truy vấn là hai số "l r": xâu con s từ vị trí l đến r có phải là xâu palindrome hay không?

**Giới hạn:**

* 1 ≤ n ≤ 5000
* 1 ≤ q ≤ 10<sup>6</sup>
* 0 ≤ l ≤ r < n

##### **Định dạng test**
**Input:**

* Dòng đầu tiên là xâu s (không chứa khoảng trắng).

**Output:**
* q dòng, mỗi dòng là 1/0 trả lời cho q truy vấn.

Ví dụ:

{% highlight text %}
abbaabba
4
0 0
0 1
0 3
1 2
---
1
0
1
1
{% endhighlight %}

##### **Thuật toán**

Gọi dp[i][j] nhận giá trị là true/false nếu xâu con từ i đến j là xâu palindrome, độ dài của xâu con này là len = j - i + 1. Ta có công thức truy hồi sau:
* Nếu len = 1: dp[i][j] = dp[i][i] = true.
* Nếu len = 2: dp[i][j] = (s[i] == s[j]).
* Ngược lại: dp[i][j] = (s[i] == s[j]) && (dp[i+1][j-1]).

Dễ thấy mảng dp được tính theo thứ tự tăng dần độ dài của xâu con.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1005 + 5;
string s;
bool dp[N][N];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // init
    cin >> s;
    int n = sz(s);

    // dp
    for(int len = 1; len <= n; len++)
        for(int l = 0; l < n - len + 1; l++) {
            int r = l + len - 1;

            // corner cases
            if (len == 1) {
                dp[l][r] = true;
                continue;
            }
            if (len == 2) {
                dp[l][r] = (s[l] == s[r]);
                continue;
            }

            dp[l][r] = (s[l] == s[r]) && dp[l+1];
        }

    // answer queries
    int q; cin >> q;
    while(q--) {
        int l, r; cin >> l >> r;
        cout << dp[l][r] << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(n<sup>2</sup> + q)

---
layout: post
title:  "Tìm xâu con palindrome dài nhất"
date: 2017-08-01 17:08:00 +0700
categories:
tags: dp palindrome
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ NKPALIN](http://vn.spoj.com/problems/NKPALIN/)

##### **Đề bài**
Một chuỗi được gọi là đối xứng (palindrome) nếu như khi đọc chuỗi này từ phải sang trái cũng thu được chuỗi ban đầu.

Yêu cầu: tìm một chuỗi con đối xứng dài nhất của một chuỗi s cho trước. Chuỗi con là chuỗi thu được khi xóa đi một số ký tự từ chuỗi ban đầu.

**Giới hạn:**

* 1 ≤ | s | ≤ 2000

##### **Định dạng test**
**Input:**

* Gồm một dòng duy nhất chứa chuỗi s, chỉ gồm những chữ cái in thường.

**Output:**
* Gồm một dòng duy nhất là một xâu con đối xứng dài nhất của xâu s. Nếu có nhiều kết quả, chỉ cần in ra một kết quả bất kỳ.

Ví dụ:

{% highlight text %}
lmevxeyzl
---
lexel
{% endhighlight %}

##### **Thuật toán**

Gọi dp[i][j] là độ dài xâu con Palindrome dài nhất trong đoạn từ vị trí i đến j, len = j - i + 1 là độ dài của đoạn này:
* Nếu len = 1: dp[i][j] = dp[i][i] = 1.
* Nếu len = 2: dp[i][j] = 1 + (s[i] == s[j]).
* Ngược lại: dp[i][j] = max(dp[i+1][j],dp[i][j-1],dp[i+1][j-1] + 2*(s[i] == s[j])).

Có được mảng dp, truy vết để in ra xâu con Palindrome dài nhất.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
string s;
int dp[2005][2005];

void trace(int l, int r) {
    if (l == r) {
        cout << s[l];
        return;
    }

    if (s[l] == s[r]) {
        cout << s[l];
        trace(l+1,r-1);
        cout << s[r];
        return;
    }

    if (dp[l][r] == dp[l+1][r]) trace(l+1,r);
    else trace(l,r-1);
}

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
            if (len == 1) { dp[l][r] = 1; continue; }
            if (len == 2) { dp[l][r] = 1 + (s[l] == s[r]); continue; }

            dp[l][r] = max(dp[l+1][r], dp[l][r-1]);
            if (s[l] == s[r]) dp[l][r] = max(dp[l][r], 2 + dp[l+1][r-1]);
        }

    // trace
    trace(0, n-1);

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N<sup>2</sup>)

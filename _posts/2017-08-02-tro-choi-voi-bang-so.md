---
layout: post
title:  "Trò chơi với băng số"
date: 2017-08-02 22:25:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ LINEGAME](http://vn.spoj.com/problems/LINEGAME/)

##### **Đề bài**
Trò chơi với băng số là trò chơi tham gia trúng thưởng được mô tả như sau: Có một băng hình chữ nhật được chia ra làm n ô vuông, đánh số từ trái qua phải bắt đầu từ 1. Trên ô vuông thứ i người ta ghi một số nguyên dương a<sub>i</sub>, i = 1, 2, ..., n. Ở một lượt chơi, người tham gia trò chơi được quyền lựa chọn một số lượng tùy ý các ô trên băng số. Giả sử theo thứ tự từ trái qua phải, người chơi lựa chọn các ô i<sub>1</sub>, i<sub>2</sub>, ..., i<sub>k</sub>. Khi đó điểm số mà người chơi đạt được sẽ là:

a<sub>i<sub>1</sub></sub> - a<sub>i<sub>2</sub></sub> + ... + (-1)<sup>(k-1)</sup>a<sub>i<sub>k</sub></sub>

Yêu cầu: Hãy tính số điểm lớn nhất có thể đạt được từ một lượt chơi.

**Giới hạn:**

* 1 ≤ n ≤ 10<sup>6</sup>
* 0 ≤ ai ≤ 10<sup>4</sup>

##### **Định dạng test**
**Input:**

* Dòng đầu tiên chứa số nguyên dương n ( n ≤ 106 ) là số lượng ô của băng số.
* Dòng thứ hai chứa n số nguyên dương a<sub>i</sub> ghi trên băng số. Các số liên tiếp trên cùng dòng được ghi cách nhau bởi ít nhất một dấu cách.

**Output:**
* Một số nguyên duy nhất là số điểm lớn nhất có thể đạt được từ một lượt chơi.


Ví dụ:

{% highlight text %}
input:
7
4 9 2 4 1 3 7
---
output:
17
{% endhighlight %}

##### **Thuật toán**

DP: Tạo mảng dp[N][2] với ý nghĩa:
* dp[i][0]: là số điểm tối đa nếu ta chọn một số lẻ các băng số trong đoạn [1, i] (kết thúc bởi việc trừ một băng số).
* dp[i][1]: là số điểm tối đa nếu ta chọn một số chẵn các băng số trong đoạn [1, i] (kết thúc bởi việc cộng một băng số).

Lấy ví dụ tại phần tử thứ i, muốn tính dp[i][0], ta có 3 trường hợp sau đây:
* Không chọn a<sub>i</sub> vào băng số, dp[i][0] = dp[i-1][0].
* Chọn duy nhất a<sub>i</sub> vào băng số, dp[i][0] = -a[i].
* Chọn a<sub>i</sub> vào băng số như là phần tử tiếp theo của băng số đã chọn trước đó: dp[i][0] = dp[i-1][1] - a[i].

Như vậy dp[i][0] là giá trị lớn nhất trong 3 trường hợp trên, hoàn toàn tương tự cho dp[i][1].

Giá trị khởi tạo: dp[1][0] = 0, dp[1][1] = a[1].

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1e6 + 5;
int n;
ll f[N][2], a[N];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input
    cin >> n;
    FOR(i,1,n) cin >> a[i];

    // dp init
    f[1][0] = 0;
    f[1][1] = a[1];

    // dp
    FOR(i,2,n) {
        f[i][0] = max(f[i-1][0], max(-a[i], f[i-1][1] - a[i]));
        f[i][1] = max(f[i-1][1], max(a[i], f[i-1][0] + a[i]));
    }
    cout << max(f[n][0], f[n][1]);

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N)

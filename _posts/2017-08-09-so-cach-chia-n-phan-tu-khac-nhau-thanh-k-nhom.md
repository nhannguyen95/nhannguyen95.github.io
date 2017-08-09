---
layout: post
title:  "Số cách chia N phần tử khác nhau vào K nhóm"
date: 2017-08-09 16:51:00 +0700
categories:
tags: dp
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ BCDIV](http://vn.spoj.com/problems/BCDIV/)

##### **Đề bài**
Cho N phần tử khác nhau, hỏi có bao nhiêu cách chia N phần tử đó thành K nhóm, mà mỗi nhóm có ít nhất 1 phần tử (các hoán vị của nhóm được xem là 1 cách).

**Giới hạn:**

* 1 ≤ K ≤ N ≤ 25

##### **Định dạng test**
**Input:**

* Dòng đầu tiên chứa số T là số test.
* T dòng tiếp theo mỗi dòng chứa 2 số N và K.

**Output:**
* T dòng, mỗi dòng là số cách với test tương ứng.

Ví dụ:

{% highlight text %}
input:
1
4 2
---
output:
7
{% endhighlight %}

Giải thích : 7 cách chia đó là (ABC)(D) , (ABD)(C) , (ADC)(B) , (DBC)(A) , (AB)(CD) , (AC)(BD) , (BC)(AD).

##### **Thuật toán**

Gọi f[i][j] là số cách chia i phần tử vào j nhóm, công thức truy hồi:

f[i][j] = f[i-1][j-1] + f[i-1][j] * j

Giải thích:
* Nếu đã chia được i-1 phần tử trước đó thành j-1 nhóm, thì phần tử thứ i sẽ chỉ có thể được chia vào nhóm thứ j.

* Nếu đã chia được i-1 phần tử trước đó thành j nhóm, thì sẽ có j cách để chia phần tử thứ i vào j nhóm đó.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
ll f[26][26];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // dp init
    f[0][0] = 1;
    FOR(i,1,25)
    FOR(j,1,i)
    f[i][j] = f[i-1][j-1] + f[i-1][j] * j;

    // answer queries
    int t; cin >> t;
    while(t--) {
        int n, k;
        cin >> n >> k;

        cout << f[n][k];
        if (t) cout << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(NK)

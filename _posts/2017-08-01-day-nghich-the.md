---
layout: post
title:  "Dãy nghịch thế"
date: 2017-08-01 17:08:00 +0700
categories:
tags: dp palindrome
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ NKINV](http://vn.spoj.com/problems/NKINV/)

##### **Đề bài**
Cho một dãy số a<sub>1</sub>.. a<sub>N</sub>. Một nghịch thế là một cặp số u, v sao cho u < v và a<sub>u</sub> > a<sub>v</sub>. Nhiệm vụ của bạn là đếm số nghịch thế.

**Giới hạn:**

* 1 ≤ n ≤ 60000
* 1 ≤ a<sub>i</sub> ≤ 60000

##### **Định dạng test**
**Input:**

* Dòng đầu là N.
* N dòng sau mỗi dòng là một số a<sub>i</sub>.

**Output:**
* Ghi trên một dòng số M duy nhất là số nghịch thế.

Ví dụ:

{% highlight text %}
input:
3
3
1
2
---
2
{% endhighlight %}

##### **Thuật toán**

Ý tưởng: tại a<sub>i</sub>, tìm số lượng các a<sub>j</sub> < a<sub>i</sub> sao cho j > i.

Từ đây có thể dùng BIT (hoặc IT) để truy vấn cũng như cập nhật số lượng các a<sub>i</sub> đã duyệt qua.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 6e4 + 5;
int n, a[N], BIT[N];

int query(int i) {
    int sum = 0;
    for(; i >= 1; i -= (i&-i))
        sum += BIT[i];
    return sum;
}

void update(int i) {
    for(; i < N; i += (i&-i))
        BIT[i]++;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // init
    cin >> n;
    FOR(i,1,n) cin >> a[i];

    // solve
    int res = 0;
    DOW(i,n,1) {
        res += query(a[i] - 1);
        update(a[i]);
    }
    cout << res;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(NlogN)

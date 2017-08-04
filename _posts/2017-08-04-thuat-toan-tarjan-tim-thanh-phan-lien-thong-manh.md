---
layout: post
title:  "Thuật toán Tarjan tìm thành phần liên thông mạnh"
date: 2017-08-04 23:13:00 +0700
categories:
tags: graph tarjan scc
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ TJALG](http://vn.spoj.com/problems/TJALG/)

##### **Đề bài**
Cho đồ thị G(V,E) có hướng n (1<=n<=10^4)  đỉnh m (1<=m<=10^5) cung, Hãy đếm số thành phần liên thông mạnh của G.

**Giới hạn:**

* 1 ≤ n ≤ 10000
* 1 ≤ m ≤ 100000

##### **Định dạng test**
**Input:**

* Dòng đầu tiên là n,m.
* M dòng tiếp theo mô tả một cung của G.

**Output:**
* Gồm một dòng duy nhất là số TPLT mạnh.

Ví dụ:

{% highlight text %}
input:
3 2
1 2
2 3
---
output:
3
{% endhighlight %}

##### **Thuật toán**

Tarjan.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
#include <iostream>
#include <cstring>
#include <vector>
using namespace std;

int n, m, num[10005], low[10005],
cnt=0, connect[10005], numSCC=0;
vector<int> a[10005], S;

void dfs(int u) {
    low[u] = num[u] = cnt++;
    S.push_back(u);
    connect[u] = 1;
    for(int v : a[u]) {
        if (num[v] == -1) dfs(v);
        if (connect[v]) low[u] = min(low[u], low[v]);
    }

    if (num[u] == low[u]) {
        numSCC++;
        while(1) {
            int v = S.back(); S.pop_back();
            connect[v] = 0;
            if (u == v) break;
        }
    }
}

int main() {

    cin >> n >> m;
    for(int i = 0; i < m; i++) {
        int u, v; cin >> u >> v;
        a[u].push_back(v);
    }

    RESET(num, -1);
    RESET(low, 0);
    RESET(connect, 0);
    for(int u = 1; u <= n; u++)
        if (num[u] == -1) dfs(u);

    cout << numSCC;

    return 0;
}
{% endhighlight %}

##### **Tính chất và ứng dụng**
* num[u]: là thứ tự duyệt dfs đến đỉnh u

* low[u]: là num nhỏ nhất trong tập những đỉnh mà u có thể đi đến.

Khởi tạo thì low[u] = num[u], low[u] sẽ thay đổi khi có một cạnh tạo nên chu trình trong đồ thị.

* Có 2 mảng để đánh dấu, num[] và connect[].

  * Mảng num[] vừa để lưu thứ tự duyệt, vừa để kiểm tra xem một đỉnh u đã được duyệt đến hay chưa

  * Mảng connect[] dùng để kiểm tra xem đỉnh v có còn được “kết nối” trong đồ thị hay không ? Nếu phát hiện ra một thành phần liên thông mạnh, và một đỉnh v có trong thành phần liên thông mạnh đó, thì ta loại đỉnh v này ra khỏi đồ thị bằng câu lệnh connect[v] = 0, điều này là quan trọng vì để tránh gây ảnh hưởng đến việc nén mảng low[] của những đỉnh khác vẫn còn nằm trong đồ thị.

##### **Độ phức tạp**
O(M + N)

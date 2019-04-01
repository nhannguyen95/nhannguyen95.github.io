---
layout: post
title:  "Tìm khớp và cầu"
date: 2017-08-04 13:03:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ GRAPH_](http://vn.spoj.com/problems/GRAPH_/)

##### **Đề bài**
Xét đơn đồ thị vô hướng G = (V, E) có n đỉnh và m cạnh. Người ta định nghĩa một đỉnh gọi là khớp nếu như xoá đỉnh đó sẽ làm tăng số thành phần liên thông của đồ thị. Tương tự như vậy, một cạnh được gọi là cầu nếu xoá cạnh đó sẽ làm tăng số thành phần liên thông của đồ thị.

Vấn đề đặt ra là cần phải đếm tất cả các khớp và cầu của đồ thị G.

**Giới hạn:**

* 1 ≤ n ≤ 10000
* 1 ≤ m ≤ 50000

##### **Định dạng test**
**Input:**

* Dòng đầu: chứa hai số tự nhiên n,m.
* M dòng sau mỗi dòng chứa một cặp số (u,v) (u<>v, 1<=u<=n, 1<=v<n) mô tả một cạnh của G.

**Output:**
* Gồm một dòng duy nhất ghi hai số, số thứ nhất là số khớp, số thứ hai là số cầu của G.

Ví dụ:

{% highlight text %}
input:
10 12
1 10
10 2
10 3
2 4
4 5
5 2
3 6
6 7
7 3
7 8
8 9
9 7
---
output:
4 3
{% endhighlight %}

##### **Thuật toán**

Định dạng Tarjan.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
#include <iostream>
#include <vector>
#include <cstring>
using namespace std;

int n, m;
vector<int> a[10005];
int cnt = 0, low[10005], num[10005], isPoint[10005];
int points = 0, bridges = 0;

void dfs(int u, int p) {
    int children = 0;
    num[u] = low[u] = cnt++;
    for(int v : a[u]) {
        if (num[v] == -1) {
            children++;
            dfs(v, u);

            // u "may" be articulation point
            if (low[v] >= num[u])
                isPoint[u] = (u == p) ? (children > 1) : 1;

            // u-v is bridges
            if (low[v] > num[u]) bridges++;

            low[u] = min(low[u], low[v]);
        } else if (v != p)
            low[u] = min(low[u], num[v]);
    }
}

int main() {

    cin >> n >> m;
    for(int i = 1; i <= m; i++) {         
        int u, v; cin >> u >> v;
        a[u].push_back(v);
        a[v].push_back(u);
    }

    memset(num, -1, sizeof num);
    memset(low, 0, sizeof low);
    memset(isPoint, 0, sizeof isPoint);
    for(int u = 1; u <= n; u++)
        if (num[u] == -1) dfs(u, u);
    for(int u = 1; u <= n; u++) points += isPoint[u];

    cout << points << ' ' << bridges;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N+M)

##### **Tính chất và ứng dụng**
num[u]: là thứ tự duyệt dfs đến đỉnh u

low[u]: là num nhỏ nhất trong tập những đỉnh mà u có thể đi đến.

Khởi tạo thì low[u] = num[u], low[u] sẽ thay đổi khi có một cạnh tạo nên chu trình trong đồ thị.

Như vậy, xét một cạnh u-v:
+ Nếu low[v] >= num[u] thì u là khớp
+ Nếu low[v] > num[u] thì u-v là cầu

**Corner case:**
Đỉnh u được chọn để duyệt dfs đầu tiên, thì với mọi v kề u, ta đều có low[v] >= num[u], trong khi nó chưa chắc đã là khớp.
Trong trường hợp này, u sẽ là khớp nếu(số_con_của_cây_DFS(u) > 1).

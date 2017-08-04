---
layout: post
title:  "Thuật toán Kruskal tìm cây khung nhỏ nhất"
date: 2017-08-03 20:23:00 +0700
categories:
tags: mst
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ QBMST](http://vn.spoj.com/problems/QBMST/)

##### **Đề bài**
Cho đơn đồ thị vô hướng liên thông G = (V, E) gồm n đỉnh và m cạnh, các đỉnh được đánh số từ 1 tới n và các cạnh được đánh số từ 1 tới m, mỗi cạnh có trọng số không âm c. Hãy tìm cây khung nhỏ nhất của đồ thị G

**Giới hạn:**

* 1 ≤ n ≤ 10000
* 1 ≤ m ≤ 15000
* 0 ≤ c ≤ 10000

##### **Định dạng test**
**Input:**

* Dòng 1: Chứa hai số n, m.
* M dòng tiếp theo, dòng thứ i có dạng ba số nguyên u, v, c. Trong đó (u, v) là chỉ số hai đỉnh đầu mút của cạnh thứ i và c trọng số của cạnh đó.

**Output:**
* Gồm 1 dòng duy nhất: Ghi trọng số cây khung nhỏ nhất.

Ví dụ:

{% highlight text %}
input:
6 9
1 2 1
1 3 1
2 4 1
2 3 2
2 5 1
3 5 1
3 6 1
4 5 2
5 6 2
---
output:
5
{% endhighlight %}

##### **Thuật toán**

Kruskal.


##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 10000 + 5;
const int M = 15000 + 5;
int n, m, lab[N];
pair<ll, ii> e[M];

int root(int u) {
    return (lab[u] < 0) ? u : lab[u] = root(lab[u]);
}

void merge(int u, int v) {
    u = root(u);
    v = root(v);
    if (u == v) return;
    if (lab[u] > lab[v]) swap(u, v);
    lab[u] += lab[v];
    lab[v] = u;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input
    cin >> n >> m;
    FOR(i,1,m)
    cin >> e[i].se.fi >> e[i].se.se >> e[i].fi;

    // dsu init
    memset(lab, -1, sizeof lab);

    // sort edges
    sort(e+1, e+m+1);

    // build mst
    ll cost = 0; // minimum cost of mst
    ll mste = 0; // edge number of mst
    FOR(i,1,m) {
        if (mste == n - 1) break;

        int u = e[i].se.fi;
        int v = e[i].se.se;
        ll c = e[i].fi;

        if (root(u) != root(v)) {
            cost += c;
            mste++;
            merge(u, v);
        }
    }

    // output
    cout << cost;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
Xấp xỉ O(MlogM + MlogN)

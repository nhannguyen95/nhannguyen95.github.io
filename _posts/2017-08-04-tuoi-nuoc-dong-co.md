---
layout: post
title:  "Tưới nước đồng cỏ"
date: 2017-08-04 13:48:00 +0700
categories:
tags: graph kuskal mst
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ FWATER](http://vn.spoj.com/problems/FWATER/)

##### **Đề bài**
Nông dân John quyết định mang nước tới cho N đồng cỏ của mình, để thuận tiện ta đánh số các đồng cỏ từ 1 đến N. Để tưới nước cho 1 đồng cỏ John có thể chọn 2 cách, 1 là đào ở đồng cỏ đó 1 cái giếng hoặc lắp ống nối dẫn nước từ những đồng cỏ trước đó đã có nước tới.

Để đào một cái giếng ở đồng cỏ i cần 1 số tiền là W_i. Lắp ống dẫn nước nối 2 đồng cỏ i và j cần 1 số tiền là P_ij.

Tính xem nông dân John phải chi ít nhất bao nhiêu tiền để tất cả các đồng cỏ đều có nước.

**Giới hạn:**

* 1 <= N <= 300
* 1 <= W_i <= 100,000
* 1 <= P_ij <= 100,000; P_ij = P_ji; P_ii=0

##### **Định dạng test**
**Input:**

* Dòng 1: Một số nguyên duy nhất: N
* Các dòng 2..N + 1: Dòng i+1 chứa 1 số nguyên duy nhất: W_i
* Các dòng N+2..2N+1: Dòng N+1+i chứa N số nguyên cách nhau bởi dấu cách; số thứ j là P_ij

**Output:**
* Dòng 1: Một số nguyên duy nhất là chi phí tối thiểu để cung cấp nước cho tất cả các đồng cỏ.

Ví dụ:

{% highlight text %}
input:
4
5
4
4
3
0 2 2 2
2 0 3 3
2 3 0 4
2 3 4 0
---
output:
9
{% endhighlight %}

Có 4 đồng cỏ. Mất 5 tiền để đào 1 cái giếng ở đồng cỏ 1, 4 tiền để đào ở đồng cỏ 2, 3 và 3 tiền để đào ở đồng cỏ 4. Các ống dẫn nước tốn 2, 3, và 4 tiền tùy thuộc vào nó nối đồng cỏ nào với nhau.

Nông dân John có thể đào 1 cái giếng ở đồng cỏ thứ 4 và lắp ống dẫn nối đồng cỏ 1 với tất cả 3 đồng cỏ còn lại, chi phí tổng cộng là 3 + 2 + 2 + 2 = 9.

##### **Thuật toán**

Thêm một đỉnh (n+1), chi phí của tất cả các đỉnh từ 1 đến n đến đỉnh mới này chính bằng chi phí đào giếng tại đỉnh đó, tức là: p[n+1][i] = p[i][n+1] = w[i].

Đáp án là tổng trọng số của cây khung nhỏ nhất của đồ thị.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
typedef pair<ll, ii> lii;
const int N = 305;
int n, lab[N];
ll w[N], p[N][N];
vector<lii> edges;

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
    cin >> n;

    FOR(i,1,n) {
        cin >> w[i];
        p[i][n+1] = w[i];
        p[n+1][i] = w[i];
    }
    FOR(i,1,n) FOR(j,1,n) cin >> p[i][j];

    // edge
    n++;
    FOR(i,1,n-1)
    FOR(j,i+1,n)
    edges.pb(mp(p[i][j], mp(i, j)));

    // kruskal init
    RESET(lab, -1);
    sort(edges.begin(), edges.end());

    // kruskal + solve
    ll cost = 0;
    int cnte = 0;
    for(auto e : edges) {
        ll p = e.fi;
        int u = e.se.fi;
        int v = e.se.se;

        if (root(u) != root(v)) {
            merge(u, v);
            cost += p;
            cnte++;
            if (cnte == n-1) break;
        }
    }

    cout << cost;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
Xấp xỉ O(MlogM + MlogN), với M = N*N/2.

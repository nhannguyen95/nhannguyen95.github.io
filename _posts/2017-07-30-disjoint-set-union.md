---
layout: post
title:  "Disjoint Set Union (DSU)"
date: 2017-07-30 13:31:00 +0700
tags:
  - algorithm
  - data structure
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[SPOJ DS2509](http://www.spoj.com/KSTN/problems/DS2509/)

##### **Đề bài**
Cho đồ thị vô hướng N đỉnh đánh số từ 1 đến N. Ban đầu tất cả đỉnh đều không có cạnh nối.

Có Q yêu cầu, mỗi yêu cầu thuộc một trong 2 dạng:
* u v 1: nối đỉnh u và v.
* u v 2: cho biết u và v có thuộc cùng một thành phần liên thông hay không? (1/0)

**Giới hạn:**

* 1 ≤ N ≤ 1e4
* 1 ≤ Q ≤ 5e4

##### **Định dạng test**
**Input:**

* Dòng đầu là Q.
* Q dòng tiếp theo, mỗi dòng có dạng u v x mô tả một trong hai yêu cầu.

**Output:**
* Với mỗi yêu cầu dạng u v 2, in ra 1 hoặc 0: u và v cùng hoặc không cùng một thành phần liên thông.

**Ví dụ:**

{% highlight text %}
input:
9
1 2 2
1 2 1
3 7 2
2 3 1
1 3 2
2 4 2
1 4 1
3 4 2
1 7 2
output:
0
0
1
0
1
0
{% endhighlight %}

##### **Thuật toán**

Disjoint Set Union (DSU).

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1e4 + 5;
int Q, lab[N];

int root(int u) {
    return lab[u] < 0 ? u : lab[u] = root(lab[u]);
}

void merge(int u, int v) {
    u = root(u); v = root(v);
    if (u == v) return;
    if (lab[u] > lab[v]) swap(u, v);
    lab[u] += lab[v];
    lab[v] = u;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // init
    cin >> Q;
    memset(lab, -1, sizeof lab);

    // dsu
    while(Q--) {
        int u, v, x;
        cin >> u >> v >> x;

        if (x == 1) merge(u, v);
        else if (x == 2) cout << (root(u) == root(v)) << '\n';;
    }

    return 0;
}
{% endhighlight %}

##### **Tính chất và ứng dụng**
* Ý nghĩa mảng lab[], xét đỉnh u trong 1 thành phần liên thông:

  * Nếu u không phải là gốc (root) thì lab[u] = cha của u.

  * Nếu u là gốc thì lab[u] < 0 và |lab[u]| = số lượng đỉnh của thành phần liên thông đó.

*Khởi tạo lab[u] = -1 với mọi u.*

* Tại mọi thời điểm, muốn truy vấn cha của u, phải dùng root(u) thay cho lab[u] (vì lab[u] lúc này có thể chưa được "nén").

* Khi merge thì ta sẽ nối thành phần liên thông có ít đỉnh hơn vào thành phần liên thông có nhiều đỉnh hơn, vì như vậy sẽ làm cho thuật toán nén đường ở hàm root tối ưu hơn.

##### **Độ phức tạp**
Hàm root : xấp xỉ O(logN).

Hàm merge : xấp xỉ O(logN).

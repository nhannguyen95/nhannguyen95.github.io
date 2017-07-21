---
layout: post
title:  "Kiểm tra chu trình trong đồ thị có hướng"
date: 2017-07-21 13:56:00 +0700
categories:
tags: dfs
no-post-nav: 0
comments: 1
---

##### **Đề bài**
Cho đồ thị có hướng n đỉnh và m cạnh, kiểm tra xem đồ thị này có chu trình hay không ?

##### **Định dạng test**
**Input:**

* Dòng đầu là n và m.
* m dòng tiếp theo là hai số u và v: có cạnh (một chiều) từ đỉnh u nối đến đỉnh v.

**Output:**
* TRUE/FALSE nếu đồ thị có chu trình / không có chu trình.

Ví dụ:

{% highlight text %}
input:
6 4
1 2
2 3
3 2
5 6
output:
TRUE
{% endhighlight %}

##### **Thuật toán**

DFS:

Nếu đồ thị có hướng G có chu trình, thì sẽ tồn tại một đỉnh u thuộc G sao cho trong quá trình duyệt cây dfs gốc u, sẽ gặp lại chính đỉnh u này.

Như vậy, với mỗi đỉnh u trong G, ta sẽ sử dụng một mảng đánh dấu visit[u] với ý nghĩa:
* visit[u] = 0 – u chưa được thăm.
* visit[u] = 1 – u “đã được thăm” trong quá trình duyệt một cây DFS gốc nào đó.
* visit[u] = 2 – u đã được thăm (quá trình duyệt cây DFS gốc u đã hoàn tất).

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
cycle = false;

void dfs(int u) {
    visit[u] = 1; // đã được thăm trong quá trình duyệt cây dfs gốc u
    for(int v : a[u])
        if (visit[v] == 0) dfs(v);
        else if (visit[v] == 1) cycle = true; // nếu gặp 1 đỉnh v đã được thăm trong quá trình duyệt cây dfs gốc u, thì đồ thị có chu trình (chu trình này nằm trong cây dfs gốc u)

    visit[u] = 2; // kết thúc quá trình duyệt cây dfs gốc u, tất cả các đỉnh con trong cây này đều được cập nhập visit = 2
}

if (cycle) đồ thị G có chu trình
{% endhighlight %}

##### **Độ phức tạp**
O(N + M)

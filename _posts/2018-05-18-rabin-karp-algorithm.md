---
layout: post
title: "Thuật toán Rabin-Karp"
date: 2018-05-18 17:49:00 +0200
categories: ['algorithm']
tags:
  - string
comments: true
mathjax: true
img:
summary: Tìm kiếm xâu sử dụng hàm băm.
---

Cho hai xâu $T$ (text) độ dài $n$ và $P$ (pattern) độ dài $m$, tìm các vị trí xuất hiện của $P$ trong $T$.

Thuật toán Rabin-Karp giải quyết bài này bằng hàm băm (hash function): biểu diễn một xâu bất kỳ bằng một số nguyên (mã băm - hash value). Khi đó, việc so sánh hai xâu qui về so sánh hai mã băm tuơng ứng: $P$ xuất hiện trong $T$ tại vị trí $i$ nếu mã băm của $P$ bằng mã băm của $T[i..(i+m-1)]$ (với $i \in [0..(n-m)]$). 

Nếu mã băm đuợc biểu diễn bằng số nguyên không quá 64 bits, độ phức tạp thời gian (time complexity) của việc so sánh xâu $P$ với một xâu con $T$ có cùng độ dài giảm từ $O(m)$ xuống $O(1)$.

Tuy nhiên mọi thứ đều có hai mặt, vấn đề của hàm băm đó là mã băm của hai xâu khác nhau có thể bằng nhau. Ví dụ xét hàm băm $hash(S)$ tính mã băm của xâu $S$ bằng cách cộng mã ASCII của các kí tự trong $S$: $hash('abcd')=97+98+99+100=394$, hàm băm này quá đơn giản và có khả năng gây trùng mã băm cao: $hash('dcba')=100+99+98+97=394$, nhưng $'abcd' \neq 'dcba'$.

Một hàm băm tốt thoả mãn các điều kiện sau:
- Tính toán nhanh.
- Xác suất trùng mã băm nhỏ.

Thuật toán Rabin-Karp xây dựng hàm băm với ý tuởng cơ số: xem mọi xâu như là một chuỗi số với một cơ số (base) nào đó. Hàm băm đuợc tính tương tự như việc ta chuyển một số nguyên về giá trị của nó, nếu là xâu kí tự thì có thể sử dụng mã ASCII (hoặc UNICODE). Một số ví dụ:
- $base = 10$, $hash('425') = 4 \times 10^2 + 2 \times 10^1 + 5 \times 10^0 = 425$.
- $base = 26$, kí tự là chữ cái từ a đến z: $hash('abc') = 97 \times 26^2 + 98 \times 26^1 + 99 \times 26^0 = 68219$.

Để tránh tràn số thì kết quả trên đuợc chia lấy dư cho một số $q$, thuờng đuợc chọn là một số nguyên tố lớn. Nếu gọi tập các kí tự được sử dụng trong xâu là $\Sigma$ thì $base$ thuờng đuợc chọn sao cho $base = \left \mid \Sigma \right \mid$ hoặc cũng là một số nguyên tố lớn. 

Dễ thấy rằng độ phức tạp thời gian để tính mã băm của xâu độ dài $k$ mất $O(k)$. Khi hiện thực thuật toán, ta sẽ "truợt" một thanh có độ dài $m$ trên xâu $T$ từ vị trí $0$ đến $n-m$ để lấy ra tất cả các xâu con độ dài $m$ của $T$ và so sánh mã băm của chúng với mã băm của $P$. Hàm băm đuợc sử dụng có một tính chất rất thú vị là nó giúp ta tính mã băm $h_i$ của xâu hiện tại $T[i..(i+m-1)]$ dựa trên mã băm $h_(i-1)$ của xâu ngay truớc đó $T[(i-1)..(i+m)]$ chỉ trong thời gian $O(1)$ thay vì tính lại trong thời gian $O(m)$. Những hàm băm có tính chất này gọi là rolling hash:
$$h_i = (base \times (h_(i-1) - base^{m-1} \times T[i-1]) + T[i+m-1]) mod q$$

Mã nguồn C++:
{% highlight c++ linenos %}
typedef unsigned long long ull;

// Returns (x^m) % q
ull mod(ull x, ull m, ull q) {
  if (m == 0) return 1;
  ull t = mod(x, m/2, q) % q;
  if (m % 2 == 0) return (t*t) % q;
  return (t*t*x) % q;
}

void rabinKarp(string & t, string & p) {
  // Assume m <= n
  int n = t.length();
  int m = p.length();
  
  ull base = 113;
  ull q = 1e9 + 7;
  ull pad = mod(10, m-1, q);
  
  ull hashP = 0;
  ull hashT = 0;
  for(int i = 0; i < m; i++) {
    hashP = ((base*hashP % q) + p[i]) % q;
    hashT = ((base*hashT % q) + t[i]) % q;
  }
  for(int i = 0; i <= n-m; i++) {
    if (hashP == hashT) {
      cout << "P appears at " << (i-m+1) << '\n';
    }
    if (i < n-m)
      hashT = (base*(hashT - pad*T[i]) + T[i+m]) % q;
  }
}
{% endhighlight %}

Độ phức tạp thuật toán:
- Time complexity: $O(m+n)$.
- Space complexity: $O(1)$.

Tham khảo:

[1]  [Thuật toán Rabin-Karp--- Rabin-Karp algorithm](http://www.giaithuatlaptrinh.com/?p=290)

[2] [Thuật toán Rabin-Karp](http://tin02ch.forums-free.com/thuat-toan-rabin-karp-t127.html)

---
layout: post
title:  "Dãy con dài nhất có tổng lớn hơn bằng p"
date: 2017-08-02 22:13:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---
##### **Bài gốc**
[SPOJ NKMAXSEQ](http://vn.spoj.com/problems/NKMAXSEQ/)

##### **Đề bài**
Cho dãy số nguyên a<sub>1</sub>, a<sub>2</sub>, …, a<sub>n</sub>.

Dãy số a<sub>i</sub>, a<sub>i+1</sub>, …, a<sub>j</sub> với 1 ≤ i ≤ j ≤ n được gọi là dãy con của dãy số đã cho và khi đó, j-i+1 được gọi là độ dài, còn a<sub>i</sub>+a<sub>i+1</sub>...+a<sub>j</sub> được gọi là trọng lượng của dãy con này.

Yêu cầu: cho số nguyên p, trong số các dãy con của dãy số đã cho có trọng lượng không nhỏ hơn p hãy tìm dãy con có độ dài lớn nhất.

**Giới hạn:**

* 1 ≤ n ≤ 50000
* -20000 ≤ ai ≤ 20000
* -10<sup>9</sup> ≤ p ≤ 10<sup>9</sup>

##### **Định dạng test**
**Input:**

* Dòng đầu tiên ghi hai số nguyên n và p cách nhau bởi dấu cách.
* Dòng thứ i trong số n dòng tiếp theo chứa số nguyên ai là số hạng thứ i của dãy số đã cho, i = 1, 2, …, n.

**Output:**
* Ghi ra số nguyên k là độ dài của dãy con tìm được (qui ước: nếu không có dãy con nào thỏa mãn điều kiện đặt ra thì k = -1.

Ví dụ:

{% highlight text %}
input:
5 6
-2
3
2
-2
3
---
output:
4
{% endhighlight %}

##### **Thuật toán**

Chưa chứng minh được tính đúng đắn của thuật toán dưới đây.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 5e4 + 5;
int n, a[N], p, f[N];
bool prefixMin[N];

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // input & init
    cin >> n >> p;
    prefixMin[0] = true;
    f[0] = 0;
    int min = 0;
    FOR(i,1,n) {
        cin >> a[i];

        // prefix sum
        f[i] = f[i-1] + a[i];

        // prefix min
        if (f[i] < min) {
            prefixMin[i] = true;
            min = f[i];
        }
    }

    // greedy
    int res = -1;
    int pos = n;
    for(int i = n; i >= 0; i--)
        if (prefixMin[i])
            while(i < pos) {
                if (f[pos] - f[i] >= p) {
                    res = max(res, pos - i);
                    break;
                }
                pos--;
            }

    // output
    cout << res;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N)

##### **Tài liệu tham khảo**
[1] [NKMAXSEQ - Le Van Dai's blogblog]( http://levandai.com/algorithm/spoj/2250-day-con-dai-nhat/)

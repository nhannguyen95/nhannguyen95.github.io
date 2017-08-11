---
layout: post
title:  "Đổi k kí tự để được xâu con liên tục cùng kí tự dài nhất"
date: 2017-07-21 13:56:00 +0700
categories:
tags: ['two pointers']
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codeforces 676C](http://codeforces.com/problemset/problem/676/C)

##### **Đề bài**
Cho xâu s độ dài n chỉ chứa kí tự 'a' và 'b'.

Tìm độ dài của xâu con liên tiếp cùng kí tự dài nhất khi thay đổi không quá k kí tự trên xâu s.

**Giới hạn:**

* 1 ≤ n ≤ 1000 000; 0 ≤ k ≤ n

##### **Định dạng test**
**Input:**

* Dòng đầu là n và k.
* Dòng thứ hai là xâu s.

**Output:**
* Độ dài dài nhất theo yêu cầu bài toán.

Ví dụ:

{% highlight text %}
input:
4 2
abba
output:
4
{% endhighlight %}

##### **Thuật toán**

Two pointers:

Giả sử đang thực hiện tối đa k thay đổi kí tự 'a' thành
'b', dễ thấy cần thay đổi tối đa k vị trí của kí tự 'a' "liên tiếp" nhau (không tính các kí tự 'b' chen giữa).

Ví dụ: s = abababa. Với k = 2, có thể thay đổi các cặp hai vị trí 'a' liên tiếp nhau như sau:

bbbbaba -> độ dài cần tìm = 4.

abbbbba -> độ dài cần tìm = 5.

ababbbb -> độ dài cần tìm = 4.

Làm tương tự khi thay đổi từ 'b' sang 'a', cập nhập đáp án tại mỗi thay đổi.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
int n, k, res = 0;
string s;

void change_to(char to) {

    int changes = 0;
    int cur_len = 0;
    int j = 0;
    REP(i,n) {
        if (s[i] == to) {
            cur_len++;
        } else {
            if (changes < k) {
                changes++;
                cur_len++;
            } else {
                while(s[j] == to) {
                    j++;
                    cur_len--;
                }
                j++;
            }
        }
        res = max(res, cur_len);
    }
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    cin >> n >> k;
    cin >> s;

    change_to('a');
    change_to('b');
    cout << res;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(N)

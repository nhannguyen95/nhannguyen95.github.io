---
layout: post
title: "Review sách: Test Driven Development: By Example, Kent Beck (2002)"
date: 2018-05-28 15:07:00 +0200
categories: [book review]
tags:
  - testing
comments: true
mathjax: true
content_level: 1
img: /images/tddbe.png
summary: Sách phù hợp cho nguời mới bắt đầu với TDD (và unit testing) và muốn biết cách áp dụng cụ thể của nó.
---

[Link sách](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530).

Không nhớ chính xác làm sao mà mình biết đến quyển này, mình tìm đọc khi đang cần học về [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD - Phát triển huớng kiểm thử) để chuẩn bị cho đợt phỏng vấn junior. 

Văn phong của sách không nghiêm túc lắm, tác giả viết như đang nói chuyện với người đọc vậy. Để đọc sách có hiệu quả, bạn nên biết sơ truớc về TDD và unit testing (không cần biết quá sâu, chỉ cần đọc qua khái niệm là ổn), và cách sử dụng JUnit (căn bản). Nguồn tham khảo thì có rất nhiều, bạn nào muốn đọc tiếng Việt thì mình recommend [trang này](https://stackjava.com/junit).

Sách chủ yếu gồm 3 chuơng chính:

- Chuơng 1: Tác giả huớng dẫn thực hành TDD với JUnit (mình recommend bạn sử dụng Eclipse) thông qua việc xây dựng một chuơng trình tên là Money. Theo mình đây là chuơng hay nhất sách: bạn sẽ nắm bắt đuợc quá trình **test/compile/run/refactor** vận hành như thế nào, làm thế nào mà quá trình này hỗ trợ và định huớng cho mã nguồn của chuơng trình, cách dẫn dắt ý tuởng thông qua test và sử dụng huớng đối tuợng của tác giả cũng rất tinh tế. Ví dụ này cũng giúp mình giải đáp một số câu hỏi ban đầu như: Có nên viết nhiều `assert` vào một test không?, hay Nên nhóm các test theo tiêu chí nào? etc.

- Chuơng 2: Tác giả huớng dẫn implement và test một unit test framework (bằng Python, nhưng ngôn ngữ không phải là vấn đề chính ở đây), giúp nguời đọc hiểu hơn về ý tuởng của các khái niệm như `test`, `setUp`, `tearDown`, `testSuite`, etc.

- Chuơng 3: Chuơng này bàn về các patterns trong TDD, chủ yếu tác giả chia sẻ kinh nghiệm và thảo luận về cách tiếp cận TDD đúng đắn: nguyên tắc thiết kế test cases, một số kỹ thuật refactor code etc. Chuơng này đọc khá chán vì không có ví dụ và từ vựng khá khó, tuy nhiên tác giả cũng chỉ ra một số yếu tố mà mình thấy quan trọng, cụ thể như: các tests phải độc lập với nhau (_"If I had one test broken, I wanted one problem. If I had two tests broken, I wanted two problems"_), và độc lập về thứ tự thực thi (việc test A đuợc thực thi truớc không ảnh huởng đến kết quả của test B đưọc thực thi sau đó, và nguợc lại).

Đánh giá: **3/5**.  Nguời mới học nên tập trung vào chuơng 1, 2 và đầu chuơng 3.

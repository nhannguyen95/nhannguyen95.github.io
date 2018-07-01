---
layout: post
title: "Review sách: Test-Driven Java Development. Viktor Farcic, Alex Garcia (2015)"
date: 2018-07-01 15:25:00 +0700
categories: [book review]
tags:
  - testing
comments: true
mathjax: true
content_level: 1
img: /images/tdjd.jpg
summary: Sách phù hợp cho nguời mới bắt đầu với TDD (và unit testing), giới thiệu về cách sử dụng các thư viện unit test trong Java cũng như best pratices cho TDD.
---

[Link sách](https://www.packtpub.com/application-development/test-driven-java-development).

Đã lâu rồi mình mới đọc lại sách của [packtpub](https://www.packtpub.com); đúng như mong đợi, sách rất dễ học và mang tính thực hành cao. Không như quyển [Test-Driven Development By Example](/review-sach-test-driven-development-by-example), người chưa biết gì về TDD cũng có thể đọc quyển này.

Nhìn chung, nội dung của sách chia làm 3 mạch chính như sau:
- Đầu tiên tác giả giới thiệu về TDD cũng như unit test.
- Những chương ở giữa sách, mỗi chương giới thiệu và thực hành về một thư viện hỗ trợ cho TDD và unit test (junit 4, mockito, etc.)
- Cuối sách là những chia sẻ của tác giả về best pratices trong TDD.

Nếu theo sát hướng dẫn trong sách; người đọc sẽ có một cái nhìn tổng quát về TDD, về cách sử dụng thư viện và có nhiều dịp để thực hành quy trình **test/compile/run/refactor**. Tuy với mỗi thư viện, tác giả đều đưa ra một ví dụ (một project nhỏ) để thực hành nó; nhưng người đọc cũng chỉ dừng lại ở mức sử dụng cơ bản mà thôi chứ không có dịp để đi sâu vào thư viện. Do đó, mình đánh giá nội dung của sách có chiều rộng hơn là chiều sâu, và mình cũng có đánh giá rằng các ví dụ mà tác giả đưa ra trong sách này không hay và tinh tế bằng quyển [Test-Driven Development By Example](/_posts/2018-05-30-review-sach-test-driven-development-by-example.md).

Để tổng kết, mình học được một trick khá thú vị đó là cách đặt tên một test method sử dụng 3 từ khoá _Given_, _When_ và _Then_ tương tự như trong BDD. Một số đoạn trích trong sách mình tâm đắc:

> **Speed is the key**: Imagine a game of ping pong (or table tennis). The game is very fast; sometimes it is hard to even follow the ball when professionals play the game. TDD is very similar. TDD veterans tend not to spend more than a minute on either side of the table (test and implementation). Write a short test and run all tests (ping), write the implementation and run all tests (pong), write another test (ping), write implementation of that test (pong), refactor and confirm that all tests are passing (score), and then repeat—ping, pong, ping, pong, ping, pong, score, serve again. Do not try to make the perfect code. Instead, try to keep the ball rolling until you think that the time is right to score (refactor).

> **It's not about testing**: T in TDD is often misunderstood. Test-driven development is the way we approach the design. It is the way to force us to think about the implementation and to what the code needs to do before writing it. It is the way to focus on requirements and implementation of just one thing at a time—organize your thoughts and better structure the code. This does not mean that tests resulting from TDD are useless—it is far from that. They are very useful and they allow us to develop with great speed without being afraid that something will be broken. This is especially true when refactoring takes place. Being able to reorganize the code while having the confidence that no functionality is broken is a huge boost to the quality.

> Requirements (specifications and user stories) are written before the code that implements them. They come first so they define the code, not the other way around. The same can be said for tests. If they are written after the code is done, in a certain way, that code (and the functionalities it implements) is defining tests. Tests that are defined by an already existing application are biased. They have a tendency to confirm what code does, and not to test whether client's expectations are met, or that the code is behaving as expected.

> A test must not only fail, but must fail for the expected reason.

> **Red Green Refactor process**
- Write a test, this test must fail (for some specific requirement or reason), we're in red.
- Run all tests and confirm the last one fails, red
- Write the code to pass the test, red
- Run all tests. green
- Refactor (optional), it's not mandatory to run this step at the end of each cycle.
- Repeat

> **Benefit of writing test first**
- Code coverage
- Focus on requirements

> What is the difference in the way we write unit tests in the context of TDD? The major differentiator is in when.

> TDD benefit: giving executable documentation that is always up to date.

> Legacy code is code without test.

Ngoài ra chương 10 còn có một số best pratices khá hay mà mình không đề cập ở trên.

Đánh giá: **3/5**.

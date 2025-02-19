---
title: "23학년도 대수능 6월 모의평가 수학 21번 Python ✏️"
date: 2022-06-10T10:02:20+09:00
draft: false
categories: ["Math"]
tags: ["python", "math"]
math: true
cover:
  image: img/june-exam-math-21/1.png
ShowToc: true
TocOpen: true
---

2022.06.09. 시행된 2023학년도 대학수학능력시험 6월 모의평가 수학 21번 문제를 프로그래밍으로 해결하는 글이다.

## 문제

![img](/img/june-exam-math-21/1.png)

자연수 n에 대하여 $4\log_{64}\left(\frac{3}{4n+16}\right)$ 의 값이 정수가 되도록 하는 1000 이하의 모든 $n$의 값의 합을 구하시오.

## 풀이

로그 밑인 64는 $2^6$ 이므로, 식을 다음과 같이 정리할 수 있다:

$$
\frac{2}{3}\log_{2}\left(\frac{3}{4n+16}\right)
$$

이 값이 정수가 되려면 $\log_{2}\left(\frac{3}{4n+16}\right)$ 의 값이 3 또는 -3의 배수가 되어야 한다. 먼저 $n$이 자연수일때 $\frac{3}{4n+16}$ 은 $2^3$, $2^6$, $2^9$ ... 가 될 수 없다. 따라서 $2^{-3}$, $2^{-6}$, $2^{-9}$ ... 가 되는 경우를 생각해보자.

$$\frac{3}{4n+16} = \frac{1}{8} = \frac{3}{24}, 4n=8, n=2$$

$$\frac{3}{4n+16} = \frac{1}{64} = \frac{3}{192}, 4n=176, n=44$$

$$\frac{3}{4n+16} = \frac{1}{512} = \frac{3}{1536}, 4n=1520, n=380$$

$$\frac{3}{4n+16} = \frac{1}{4096} = \frac{3}{12288}, 4n=12272, n=3068$$

$$ ... $$

$\log_{2}\left(\frac{3}{4n+16}\right)$ 이 -12가 되면 $n$의 값은 1000을 초과한다. 따라서 1000 이하의 자연수 $n$의 값의 합은 $2+44+380=426$이다.

## 코드

```py {linenos=true}
import math
res = 0
for n in range(1, 1001):
    val = 2/3*math.log2(3/(4*n+16))
    if val == int(val):
        res += n
print(res)
```

어떤 수 $x$가 정수인지 판별하려면 $x$와 $x$를 정수형 타입으로 바꾼 값이 일치하면 $x$는 정수라고 말할 수 있다.

![img](/img/june-exam-math-21/2.png)

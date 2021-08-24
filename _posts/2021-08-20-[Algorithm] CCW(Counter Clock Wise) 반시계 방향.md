---
layout: post
title: "[알고리즘 Algorithm] CCW (Counter-Clock-Wise) 판정"
categories: ['Algorithm']
comments: true
---
<script type="text/javascript" 
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML">
</script>
### CCW (Counter - Clock - Wise)

CCW는 말 그대로 반시계 방향이라는 의미이다. CCW 알고리즘은 어떤 점들의 순서가 반시계방향으로 주어져 있는지 판별하는 알고리즘이다.

<p align = "center"> <img src="\assets\img\CCW\ccw.png" alt="ccw" style="zoom: 50%;" /> </p>

선분 $$ \small \overline{AB} $$와 $$ \small \overline{BC} $$가 이루는 각을 $$ \small \theta $$라고 해보자. 그럼, $$ \small \theta $$가 $$ \small 0 \sim \pi $$ 사이의 값을 가질 때, A - B - C가 이루는 방향이 반시계 방향임을 알 수 있다. 그러면, $$ \scriptsize \overrightarrow{AB} $$와  $$ \scriptsize \overrightarrow{BC} $$가 이루는 각을 생각해 보자. 이는, $$ \small \pi $$에서 $$ \small \theta $$를 뺀 값인 $$ \small \pi - \theta $$ 이다. $$ \small \theta $$가 $$ \small 0 \sim \pi $$ 이면, $$ \small \pi - \theta $$값 역시도 같은 범위에 속하므로, 두 벡터가 이루는 각을 살펴보아도 반시계 방향 판정이 가능하다.

그럼 그 각을 어떻게 알아낼 수 있을까? 벡터 간 연산에는 내적과 외적이 있었다. 내적은 $$ \small \overrightarrow{a} \cdot \overrightarrow{b} = \mid a \mid \mid b \mid cos \theta $$로 정의 되므로, $$ \small \theta $$에 대해 정리하면 각을 구할 수 있다. 그러나, 이 방법은 아크코사인값을 근사로 구해야하므로 오차가 실수 오차가 발생할 수 있을 뿐만 아니라 복잡하다. 더 쉬운 방법은 외적을 이용하는 방법이다. 

외적의 크기는 $$ \small \mid \overrightarrow{a} \times \overrightarrow{b}\mid = \mid a \mid \mid b \mid sin \theta $$ 이다. 이때, 사인값은 $$ \small 0 \sim \pi $$ 사이의 각에서 양수값을 가진다. 즉, 두 벡터가 이루는 각에 대한 사인값이 양수값이라면 반시계 방향임을 훨씬 더 간단하게 알 수 있다. 정리하자면, 두 벡터간 외적값에 따라 방향을 결정할 수 있다는 것이다.

외적값이 **양수라면 CCW**를 의미하고, 0이라면 평행하므로 직선상에 점들이 존재하는 것이고, 음수라면 CW인 것이다. 이를 코드로 짠다면, 단순히 다음처럼 짤 수 있을 것이다.

```c++
typedef struct {
    long long x;
    long long y;
} Point;

int ccw(Point& v, Point& u) { //v vector and u vector
    long long crossProduct = v.x * u.y - v.y * u.x;
    
    if(crossProduct > 0) return 1;
    else if(crossProduct < 0) return -1;
    else return 0;
}
```

나는 벡터를 다루기 위해 구조체를 만들었고, ccw함수에선 두 벡터를 받아 외적을 계산한 후 부호에 따라 리턴해주면 된다.

이 알고리즘은 **Convex Hull**을 찾을 때도 사용되고, 어떤 점이 **다각형 내부에 있는지 판정**하는지에도 사용할 수 있다.

이걸로 풀 수 있는 가장 쉬운 문제는 [백준 11758 CCW](https://www.acmicpc.net/problem/11758) 문제이다. 문제 이름처럼 그냥 CCW를 구현하기만 하면 되는 매우 간단한 문제이다.
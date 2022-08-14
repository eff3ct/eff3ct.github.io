---
layout: post
title: "Convex Hull - Monotone Chain 시각화"
categories: ['General']
comments: true
---
<script type="text/javascript" 
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML">
</script>
### Manim Library
[Manim](https://www.manim.community/) 이라는 수학 애니메이션 **visualization을 도와주는 라이브러리**가 있길래 한 번 써봤다. 아마 알 사람들은 다 알만한 유튜버 [3blue1brown](https://www.youtube.com/c/3blue1brown)가 개발한 라이브러리다. 왠만한 수학 관련 오브젝트들을 지원하고 사용법도 쉬워서 조금만 배워도 뭘 만들 수 있다. 동아리 학술부장이 '컨벡스헐 ㄱ' 라고 해서 진짜로 만들었다.

<p align="center">
<img src = "assets/img/convex-hull-monotone-chain/1.png" alt = "1">
</p>

### Monotone Chain을 이용한 Convex Hull 시각화
사실 Graham's Scan으로 만드려고 했는데.. 파이썬에 그닥 익숙하지 않아서 커스텀 소트 함수를 제대로 못짰다. 그래서, 타협을 본게 Monotone Chain이다. 아래껍질과 윗껍질을 각각 찾아 합치는 방법이니 그걸 잘 보여주려고 색깔도 다르게 하고 스택에 들어가고 pop하는 과정이 나타나도록 색깔로 구분해서 표현해봤다.

```python
for i in range(n):
    while len(low_hull) >= 2:
        p1, p2 = low_hull[-2], low_hull[-1]

        if self.ccw_point(p1, p2, p[i]) > 0:
            break

        low_hull.pop()
        if len(line_obj) >= 1:
            line = Line(p2 + [0], p[i] + [0], color = RED_C)
            self.play(Create(line), run_time = delta)
            self.play(FadeOut(line), run_time = delta)
            self.play(FadeOut(line_obj[-1]), run_time = delta)
            line_obj.pop()

    low_hull.append(p[i])
    if len(low_hull) >= 2:
        p1, p2 = low_hull[-2], low_hull[-1]
        line = Line(p1 + [0], p2 + [0], color = PINK)
        line_obj.append(line)
        self.play(Create(line), run_time = delta)
```
대충 요런 코드 가지고 만드니까 잘 만들어짐! 아래는 결과물이다.

<video autoplay loop muted width = "600">
    <source src = "assets/vid/convex-hull-monotone-chain/ret.mp4" type = "video/mp4">
</video>

리액트 공부는 언제하지..?
---

layout: post
title: "[알고리즘 Algorithm] 위상정렬(Topology Sort)"
categories: ['Algorithm']
comments: true
---
<script type="text/javascript" 
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML">
</script>

### 위상 정렬(Topology Sort)

위상정렬은 노드간 위상에 따라 노드들의 순서를 정하는 정렬방법이다. 무슨 소리인지는 아래의 그림으로 알아보자

<p align = "center"> <img src="\assets\img\topologysort\sort.png" alt="sort"/> </p>

그래프를 보면, 크게 **1 2 3 5**와 **4 3**의 순서를 가지는 것을 알 수 있다. 이를 순서대로 표시하면, 위 sort에 표시한 순서들 중 하나로 표시할 수 있겠다. 즉, 위상정렬은 간단하게 말하자면, 어떤 **DAG(Directed Acycle Graph, 유향 무사이클 그래프)**에서 **노드의 순서를 찾는 방법**이다. 당연하게도 무향 그래프이거나, 사이클이 있으면 위상으로 정렬하기 어렵기에 위상정렬은 **DAG**에 한정된다.

이런것을 어디에 쓰냐면, 순서를 지켜서 관계를 이루어야만 하는 문제들에 쓰인다. BOJ의 [2252 줄세우기](https://www.acmicpc.net/problem/2252) 같은 문제를 이런 위상정렬을 사용해 풀어낼 수 있다. ([->2252번 풀이보기](https://eff3ct.github.io/boj/2021/09/09/BOJ-2252-줄-세우기/)) 위상정렬은 **큐를 활용**해서 구현할 수도 있고, **스택을 활용**해서 구현할 수도 있다. 여기에서는 간단하게 **큐를 사용**해서 푸는 방법을 알아보자.

> **1. 어떤 노드 X가 있을때, 그 노드로 연결되는 간선이 없는 노드들을 큐에 push한다.**
>
> **2. 큐에서 노드를 꺼내 정렬됐음을 표시하고, 그 노드와 연결되어 있는 다른 노드들과의 간선을 끊어준다.**
>
> **3. 연결되어 있었던 다른노드들 중에서 연결되는 간선이 없는 노드를 찾아 큐에 push한다.**
>
> **4. 2번 ~ 3번 과정을 노드수만큼 반복한다.**

이 과정을 따라가면, 위상순으로 정렬된 노드들을 얻을 수 있다. 그림으로 알아보자.

<p align = "center"> <img src="\assets\img\topologysort\1.png" alt="l"/> </p>

아무것도 연결되지 않은 **1**과 **4**를 queue에 push한다. (1번 과정)

<p align = "center"> <img src="\assets\img\topologysort\2.png" alt="2"/> </p>

queue에서 **1**을 꺼내 연결된 간선을 지워주고 아무것도 연결되지 않은 노드인지 검사해 queue에 push한다. 이 경우에는 **2**가 아무것도 연결되지 않은 노드가 되므로 **2**를 queue에 push한다. 그 다음, **4**를 꺼내 마찬가지의 과정을 진행해주면 위 그림처럼 된다.

<p align = "center"> <img src="\assets\img\topologysort\3.png" alt="3"/> </p>

<p align = "center"> <img src="\assets\img\topologysort\4.png" alt="4"/> </p>

같은 과정을 이렇게 반복해 주면, **queue가 모두 비는 시점에 ret에 topology sort의 결과가 저장된다.**

이렇게 짜기 위해서는 연결되어 있는 노드 수가 몇개인지 알고 있어야 하기에 그래프를 입력받을때 아래의 코드처럼 따로 해당 노드에 연결되어 있는 개수를 세어주어야한다.

```c++
//N은 노드의 개수, M은 간선의 수
vector<vector<int>> adj(N + 1, vector<int>()); //인접 리스트
vector<int> count(N + 1, 0); //노드에 몇개가 연결되어 있는지

for(int i = 0; i < M; ++i) {
    int u, v; cin >> u >> v;
    adj[u].push_back(v); //u -> v로 연결되는 간선
    count[v]++; //v노드에 u 하나가 연결되어있는 것이니 +1 해준다.
}
```

이렇게 데이터를 받아주고 나면 위상정렬을 할 준비가 완료된 것이다. 그냥 위의 과정을 그대로 코드로 구현하면 되기에 아래와 같이 짜줄 수 있다.

```c++
void topologySort(vector<vector<int>>& adj, vector<int>& count) { //인접리스트와 연결된 노드수를 파라미터로 받는다
    vector<int> ret; //결과를 저장할 벡터
    queue<int> q; 
    for(int i = 1; i <= N; ++i) {
        if(count[i] == 0) q.push(i); //연결된 개수가 0개이면 queue에 push
    }

    for(int i = 0; i < N; ++i) { //노드 수만큼 반복해주면 된다.
        int now = q.front(); //queue에서 원소를 꺼내고 pop한다.
        q.pop();

        ret.push_back(now); //그 원소는 ret에 push한다.
        
        for(auto& next : adj[now]) { 
            if(--count[next] == 0) q.push(next); //간선을 지우고(count 감소 시키는걸로 충분하다.) 그 값이 0이면 queue에 push
        }
    }

    for(auto& element : ret) cout << element << ' '; //출력
}
```

BOJ에서 위상정렬을 활용해 풀만한 문제는 위에서 소개한 [2252 줄세우기](boj.kr/2252)와 [1005 ACM Craft](boj.kr/1005)정도가 있을 듯 하다. 전자는 그냥 위상 정렬 그 자체로 풀 수 있고, 후자는 위상정렬 + 간단한 DP로 풀 수 있다. 나는 후자가 더 어렵다고 생각하는데.. 왜인진 모르겠으나 티어는 전자가 더 높다. 여튼, BFS나 DFS로만은 해결할 수 없는 그런 문제들을 해결할 수 있는 그래프 분석 도구이다. 구현하기 복잡하지 않고 쉽기 때문에, 알아두면 좋을 듯 하다.
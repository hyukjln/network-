# Network-Science-Final-Exam

이 코드는 네트워크 이론의 voter model, ER model, configuration model, chung-lu model, BA model 과 degree distribution, mean geodesic distance,  giant_component 등에 대해서 다룬다. 

---

## **필요한 라이브러리**
```python
import networkx as nx
import matplotlib.pyplot as plt
import random as rd
import itertools
import numpy as np
import math
```
---

## **함수 실행 방법(colab 기준)**

* 하나의 함수만 실행 할때
```python
!git clone https://github.com/namu10664/Network-Science-Final-Exam.git

%cd Network-Science-Final-Exam

from network_project_function import (함수이름)
```
* 모든 함수를 실행 할때
```python
!git clone https://github.com/namu10664/Network-Science-Final-Exam.git

%cd Network-Science-Final-Exam

import network_project_function
```
---

## **함수 설명**(자세한 실행 방법은 .ipynb파일을 참조)

**gnp_random_graph(N, p)**
  
* ER 모델, 무작위 연결 패턴을 가진 네트워크로 모든 노드 쌍이 동일한 확률로 연결된다.

* 실행에 필요한 함수: 없음

  * N: 노드의 수
  * p: 연결 확률
    
* 반환값: G(네트워크)(type: networkx.classes.graph.Graph)

**voter_model_on_er_multiple_runs(n=100, p=0.1, initial_opinion_ratio=0.1, opinion_change_prob=0.1, max_steps=500, num_runs=1, draw_network_step=None)**
  
* voter 모델, 네트워크의 각 노드가 무작위 단계로 선택되고 노드와 연결된 이웃 노드 중 하나를 일정 확률로 이웃의 의견을 자신의 의견으로 복사된다.

* 실행에 필요한 함수: gnp_random_graph(N, p), draw_network_graph

  * n=100: 노드의 수
  * p=0.1: 의견이 복사 될 확률
  * initial_opinion_ratio=0.1: 초기 의견1의 비율
  * opinion_change_prob=0.1: 의견이 복사될 확률
  * max_steps=500: 최대 시행 횟수
  * num_runs=1: voter_model의 반복 횟수
  * draw_network_step=None: 네트워크 그림이 그려지는 간격 (입력하지 않으면 실행되지 않음)

* 반환값: 없음 (그래프 출력)

**draw_network_graph(G, step, fixed_positions)**
  
* voter_model_on_er_multiple_runs함수에서 draw_network_step이 None이 아닐때 실행되는 함수로, 네트워크에서 의견0은 파란색으로, 의견1은 빨간색으로 나타낸다.

* 실행에 필요한 함수: 없음

  * G: 네트워크(voter model)
  * step: 현재 step 수
  * fixed_positions: 네트워크에서 노드의 위치
   
* 반환값: 없음 (그래프 출력)

**mean_degree_c(p,n)**

* 평균 차수(c)를 계산한다.

* 실행에 필요한 함수: 없음

  * p: 연결 확률
  * n: 노드의 수

* 반환값: 평균 차수(c)(type: list)

**Poisson(m,n,p)**

* 포아송 분포 그래프 함수

* 실행에 필요한 함수: mean_degree_c(p,n)

  * m: degree
  * n: 노드의 수
  * p: 연결 확률

* 반환값: m,n,p에 따른 포아송(y)값(type: float)

**er_ensemble(n,p, ensemble)**

* ER 모델을 ensemble만큼 반복하여 degree distribution의 x(list), y(list), 포아송그래프y(list)를 반환한다.

* 실행에 필요한 함수: gnp_random_graph(n,p), Poisson(m,n,p)

  * n: 노드의 수
  * p: 연결 확률
  * ensemble: ensemble 반복 횟수

* 반환값: ER_x_list, ER_y_list, poisson_y_list (type: list, list, list)

**ER_dist_poisson(set_values, ensembles)**

* er_ensemble함수를 각각 다른 조건으로 실행한다.

* 실행에 필요한 함수: er_ensemble(n,p,ensembles)

  * set_values: [[노드의 수(N), 연결 확률, 그래프 색상],...]의 형태로 입력ex) [[100,0.01,'C0'],[300,0.01,'C1'],[800,0.01,'C2']]
  * ensemble: ensemble 반복 횟수

* 반환값: 없음 (그래프 출력)

**er_avg_dis(N, p, ensemble, BINS=0.01)**

* ER 모델의 mean geodesic distance를 히스토그램의 형태로 반환한다.

* 실행에 필요한 함수: gnp_random_graph(N,p)

  * N:노드의 수
  * p: 연결 확률
  * ensemble: ensemble 반복 횟수
  * BINS=0.01: 히스토그램의 bins의 간

* 반환값: histogram, bin_edges, Average Mean Geodesic Distance (type: numpy.ndarray, numpy.ndarray, numpy.float64)

**DFS(G, u, visited = None)**

* 그래프에서 componant를 구하기 위해 DFS를 사용한다.

* 실행에 필요한 함수: 없음

  * G: 네트워크(엣지 딕셔너리 형태. er_giant_component함수 참고)
  * u: DFS 시작 노드
  * visited = None: 방문한 노드

* 반환값: visited (type: list)

**er_giant_component(N, p_max, p_step, ensemble)**

* 주어진 p의 범위에서 Giant Component의 크기를 순서대로 리스트로 반환한다.

* 실행에 필요한 함수: gnp_random_graph(N,p), DFS(G, u, visited = None)

  * N: 노드의 수
  * p_max: 최대 확률
  * p_step: 확률의 증가 단계
  * ensemble: ensemble 반복 횟수

* 반환값: Giant_Component_size_list (type: list)

**visualize_componant(N=100, p=0.1)**

* N,P로 생성된 한 ER모델의 Giant_Component의 크기와 네트워크를 반환한다.

* 실행에 필요한 함수: gnp_random_graph(N,p), DFS(G, u, visited = None)

  * N:노드의 수
  * p: 연결 확률

* 반환값: 없음 (그래프 출력)

**visualize_network_ER(p=0.1, n=100, ensemble=100)**

* ER모델의 네트워크와 ensemble만큼 반복한 후의 degree distribution 출력

* 실행에 필요한 함수: gnp_random_graph(N,p)

  * p=0.1: 연결 확률
  * n=100: 노드의 수
  * ensemble=100: ensemble 반복 횟수

* 반환값: 없음 (그래프 출력)

**create_config_graph(degree_seq)**

* configuration model. 주어진 degree_seq를 기반으로 연결이 있는 노드들을 연결 수만큼 무작위로 연결한다.

* 실행에 필요한 함수: 없음

  * degree_seq: degree_sequence

* 반환값: G(네트워크)(type: networkx.classes.graph.Graph)

**visualize_network_config(degree_seq,ensemble=100)**

* configuration 모델의 네트워크와 ensemble만큼 반복한 후의 degree distribution 출력

* 실행에 필요한 함수: create_config_graph(degree_seq)

  * degree_seq: degree_sequence
  * ensemble=100: ensemble 반복 횟수

* 반환값: 없음 (그래프 출력)

**create_chung_lu_net(degree_seq)**

* chung-lu model. 주어진 degree_seq를 기반으로 "(i의 degree*j의 degree)/degree의 수" 를 확률로 i,j를 연결한다.

* 실행에 필요한 함수: 없음

  * degree_seq: degree_sequence

* 반환값: G(네트워크)(type: networkx.classes.graph.Graph)

**visualize_network_chung_lu(degree_seq,ensemble=100)**

* chung-lu 모델의 네트워크와 ensemble만큼 반복한 후의 degree distribution 출력

* 실행에 필요한 함수: create_chung_lu_net(degree_seq)

  * degree_seq: degree_sequence
  * ensemble=100: ensemble 반복 횟수

* 반환값: 없음 (그래프 출력)

**ba_model(n, m)**

* BA model. 네트워크에서 엣지수에 비례해서 새로운 노드가 연결 확률이 늘어난다.

* 실행에 필요한 함수: 없음

  * n: 노드의 수
  * m: 새로 생성된 노드의 연결 수

* 반환값: G(네트워크)(type: networkx.classes.graph.Graph)

**visualize_ba_network(n=100, m=2)**

* BA 모델의 네트워크와 ensemble만큼 반복한 후의 degree distribution 출력

* 실행에 필요한 함수: ba_model(n, m)

  * degree_seq: degree_sequence
  * ensemble=100: ensemble 반복 횟수

* 반환값: 없음 (그래프 출력)

**generate_random_networks(degree_seq, n=100, p=0.1, m=2)**

* ER 모델, configuration 모델, chung-lu 모델, BA 모델을 한번에 출력한다.

* 실행에 필요한 함수: gnp_random_graph(n, p), create_config_graph(degree_seq), create_chung_lu_net(degree_seq), ba_model(n, m)

  * degree_seq: degree_sequence
  * n=100: 노드의 수
  * p=0.1: 연결 확률
  * m=2: 새로 생성된 노드의 연결 수

* 반환값: 없음 (그래프 출력)

**create_CCDF(G)**

* 네트워크의 CCDF를 리스트의 형태로 반환한다.

* 실행에 필요한 함수: 없음

  * G: 네트워크

* 반환값: ccdf_list (type: list)

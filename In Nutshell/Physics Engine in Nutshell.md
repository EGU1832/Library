# Table of Contents

- [1. What is Physics Engine?](#1-what-is-physics-engine)
- [2. Types of Physics Engine](#2-types-of-physics-engine)
- [3. Descriptions](#3-descriptions)
- [4. Physics](#4-physics)
- [5. Mathematics](#5-mathematics)
- [6. References](#6-references)


# 1. What is Physics Engine?
- 물리 엔진은 컴퓨터 그래픽, 비디오 게임 및 영화 (CGI) 영역에서 사용되는 물리적 시스템의 대략적인 시뮬레이션을 제공하는 컴퓨터 소프트웨어이다.
- 강체 역학 (충돌 감지 포함), 연체 역학 및 유체 역학


# 2. Types of Physics Engine

Real-time | High-precision
:---:|:---:
저성능 | 고성능
실시간 | 정밀
비디오게임, 인터렉티브 컴퓨팅 | 과학 시뮬레이션, 애니메이션


# 3. Descriptions
## 3.1 Game engines
- 성능이 정확도보다 중시되기 때문에 Real-time 물리 엔진을 사용하나, 정확한 물리적 측정이 필요한 경우에는 근사치로 실제 물리학을 따른다.
- 지각적으로 정확한 근사치를 제공하는 데 초점을 둔다.
	- Semi-implicit euler method: 상용 미분 방정식을 수치적으로 해결하기 위한 방법 중 하나, 해석적으로 어려운 문제에 대해 근사적인 해를 얻을 수 있다.
- 동작 방식
	1. 초기 조건 설정: 변수들의 초깃값 설정
	2. 시간 단계 설정: △t 결정
	3. 시간에 따른 반복: 초기 단계 → 초기 단계 + △t
	4. 변수 암시적으로 업데이트: iterative solver (ex. Newthon's method)
	5. 변수 명시적으로 업데이트: 암시적인 변수를 얻은 후, 이전 시간 단계의 값 + 암시적 변수
	6. 종료 시간에 도달할 때까지 4. 5. 반복

## 3.2 Collision detection(충돌 감지)
- Mesh: 복잡하고 상세한 모양 (꽃병, 나뭇가지 등)
- Shape: 간단한 모양 (실린더, 구, 정육면체 등)
- Collision Geometry: processing을 위해 복잡한 mesh대신 단순한 형태의 mesh를 사용하는 것
	- Sphere: 구
	- Convex hull: 볼록한 선체 <br> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/Extreme_points.svg/1024px-Extreme_points.svg.png" width="20%" height="20%"/>
- 프레임 수에 정밀도가 영향을 받는다. Second Life는 이 결함을 극복하기 위해 mesh를 보이지 않는 화살표가 있는 발사체로 표현한다.
	- Bullet
	- Havok

## 3.3 Soft-body dynamics
- Bounding-box 기반의 물리 시스템 대신 finit element 기반의 시스템을 사용한다.
- 이 시스템에서는 3차원 기반의 tessellation(테셀레이션)을 이용한다. <br/>
<img src="https://i.namu.wiki/i/2tNflvmSiAfsQSfDezK2Kv62alfrjbH2I1zFo4Lfu7SQGYJK1cX1ccRZZJMVbd4SzNwUnaq9A0NCAbBHedDtEcHOMkSlAOEjccmxRC9lmsRZ6YIqsih6xxstVFZub8sbpyG9kpKYR-pX6P96igZmOQ.webp" width="50%" height="50%"/><br/>
- Tessellation은 물리적 특성(toughness, platicity 등)을 가지고 있는 여러 개의 finite element로 구성된다. 예를 들어 3d 애니메이션에서 캐릭터를 모델링한 요소를 보면 복잡한 형태의 모양이 여러개의 단순한 모양으로 이루어져 있는것을 볼 수 있다.
- 이러한 특성상 높은  사양의 CPU혹은 GPU를 요구한다.
- 이 finite element들은 solver(솔버)에 의해 3D 객체의 stress(응력)을 모델링하는데 사용된다. 이 stress는 현실적인 물리적 파괴 또는 변형을 표현하는데 사용된다. 이 finite element의 개수가 늘어날 수록 더 정확한 물리 시뮬레이션이 가능하다.
- 이 3D객체의 시각적 표현은 CPU나 GPU 자체의 내부의 deformation shader(셰이더)를 통해 이루어진다.

## 3.4 Brownian motion(브라운 운동)
- 브라운 운동이란 액체나 기체 속에서 미소입자들이 불규칙하게 운동하는 현상을 말한다. 여기서는 실제 세계에서는 물체들이 실제로 미세하게 움직이고 있다는 것으로 이해하겠다.
- Game Physics에서는 모든 물체의 미세한 움직임을 항상 계산하기에는(jitter) CPU에 무리가 가기 때문에, 일정 시간 동안 특정 거리 이상 움직이지 않은 객체의 물리 계산을 비활성화하는데, 이를 "sleep", 즉 휴면 상태라고 한다.

## 3.5 Paradims
- Game Physics 시스템의 구성요소
	- Collision Detection/Collision Response: 충돌 감지 및 응답
	- Dynamics Simulation: 개체에 영향을 미치는 힘 해결
	- Fluid Simulation: 유체 시뮬레이션
	- Animation Control System: 애니메이션 제어 시스템
	- Asset Integration Tools: 통합 도구
- 물체의 물리적 시뮬레이션의 세 가지 주요 패러다임
	- Penalty Method: 상호작용을 일반적으로 질량-스프링 시스템으로 모델링하는 방법, 강체가 아닌 물체나 천막과 같은 유연한 구조물을 시뮬레이션할 때 주로 사용된다.
	- Constraint Based Methods: 물리적 법칙을 추정하는 제약 방정식을 해결하는 방법, 물체 간의 충돌, 관절 제약, 연결 제약 등을 고려하여 물리적 동작을 모델링할 때 주로 사용된다.
	- Impulse Based Methods: 물체 간의 충돌이나 상호작용에 충격을 적용하는 방법, 물체의 운동량, 에너지, 강체의 회전 등을 모델링할 때 주로 사용된다.
	- Hybrid Method: 위의 패러다임들의 측면을 결합한 방법, 정 상황에 따라 다양한 물리 시뮬레이션 요구사항을 충족시키기 위해 사용된다.

# 4. Physics

# 5. Methematics

# 6. References
- [Game physics - Wikipedia](https://en.wikipedia.org/wiki/Game_physics)
- 
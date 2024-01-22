# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Math Basics](#2-math-basics)
- [3. Modeling](#3-modeling)
	* [3.1 Polygon Mesh](#31-polygon-mesh)
	* [3.2 Vertex Representation](#32-vertex-representation)
	* [3.3 Surface Normals](#33-surface-normals)
	* [3.4 Export and Import](#34-export-and-import)
- [4. Spaces and Transforms](#4-spaces-and-transforms)
	* [4.1 Homogeneous Coordinates](#41-homogeneous-coordinates)
	* [4.2 Affine Transform](#42-affine-transform)
	* [4.3 World Transform](#43-world-transform)
	* [4.4 Rotation and Object-space Basis](#44-rotation-and-object-space-basis)
	* [4.5 Inverse of 3D Affine Transform](#45-inverse-of-3d-affine-transform)

# 1. Introduction

# 2. Math Basics

# 3. Modeling
## 3.1 Polygon Mesh

- 정점(vertex):
	공간 상에서의 위치(position)를 나타내는 것으로, 일반적으로 여러 개가 모여서 특정 형태의 오브젝트를 구성하는 요소를 정점 이라고 한다.
	reference ▶ [vertex](https://hellowoori.tistory.com/31)
- 폴리곤(polygon):
	3개의 정점이 모이면 하나의 면(face)을 만들 수 있다. 이렇게 3개의 정점으로 만들어진 삼각형을 폴리곤이라고 한다. 삼각형은 3D 물체의 기본 구성 요소이다.
- 변(edge):
	폴리곤에서 정점과 정점을 연결하는 직선을 변 또는 모서리라고 한다.
- 메시(mesh):
	폴리곤이 모여서 하나의 3차원 물체를 만들게 되는데 이것을 메시라고 한다. 즉, 메시는 폴리곤이 모여서 만들어진 3차원 공간상의 객체(object)이다.
- LOD:
	Polygon mesh의 정점의 개수
	Reolustion, level of detail 라고도 표현한다.

## 3.2 Vertex Representation

- Vertex array:
	정점은 enum화 된 배열의 형태로 메모리에 저장된다.
	정점을 메모리에 저장하는 방식에는 두 가지가 있다.

### 3.2.1 Non-indexed Represantation

### 3.2.2 Indexed Representation
## 3.3 Surface Normals

- Surface normal은 컴퓨터 그래픽스에서 중요한 역할을 한다.
- Triangle normal과 vertex normal이 있다.

### 3.3.1 Triangle Normal

- 삼각형 < $p_1$, $p_2$, $p_3$ >에서 $v_1$은 $p_1$과 $p_2$를 연결한 벡터이고, $v_2$은 $p_1$과 $p_3$를 연결한 벡터일 때, Triangle normal의 식은 다음과 같다.
$$n = \frac{v_1 \times v_2}{\lVert v_1 \times v_2 \rVert}$$
- 보통의 경우 $p_1$, $p_2$, $p_3$는 CCW(counter-clockwise) 순으로 나열되어 있다. 즉, normal이 다면체의 바깥을 향한다.
- Polyhedron(다면체):
	평면 다각형들로 구성된 3차원 도형이다.

### 3.3.2 Vertex Normal

- Vertex normal은 각각의 정점에 부여되는 normal로, 정점을 포함하는 polygon들의 triangle normal의 평균이다. Vertex normal의 식은 다음과 같다.
$$n = \frac{n_1 + n_2 + n_3 + n_4 + n_5 + n_6}{\lVert n_1 + n_2 + n_3 + n_4 + n_5 + n_6 \rVert}$$
- Vertex array의 필수 구성 요소이며, Max같은 modeling package에서 자동으로 만들어진다.

## 3.4 Export and Import

- Polygon mesh들은 3ds Max같은 graphics package들로부터 만들어지고, 파일로 저장되어 game engine과 같은 3d application program들로 보내진다.
	* Export: 다른 프로그램이 이해할 수 있는 언어로 변환하여 내보내는 과정이다.
	* Import: 변환된 data를 다른 프로그램에서 받아들이는 과정이다.
- 가장 많이 쓰이는 파일 포멧은 .obj이다.

### 3.4.1 Export

- Export file에는 다음과 같이 정보가 저장되어 있다.

| Array | v | vn | f |
| :--: | :--: | :--: | :--: |
| Components | vertex | vertex normal | face (ex. triangle) |
| Format | [ x, y, z ] | [ x, y, z ] | [ v//vn, v//vn, v//vn ] |
| 중복 허용 | X | X | O |
| 개수 | 정점의 개수 | 정점 normal의 개수 | polygon의 개수 |

### 3.4.2 Import

- Import file에는 다음과 같이 정보가 저장되어 있다.

| Array | vertex array | index array |
| :--: | :--: | :--: |
| Components | vertex, vertex normal | index data of faces |
| Format | v [ x, y, z ], vn [ x, y, z ] | index number |
| 중복 허용 | X | O |
| 개수 | 정점의 개수 | polygon의 개수 $\times$ polygon의 꼭짓점의 개수 |

# 4. Spaces and Trasforms
## 4.1 Homogeneous Coordinates

- Homogeneous coordinates(동차 좌표)는 컴퓨터 그래픽스에서 점, 벡터 및 변환을 표현하는 데 사용되는 좌표 시스템으로, 우리가 흔히 사용하는 카테시안 좌표 시스템과는 차이가 있다.
- 동차 좌표를 이용하면 transformation matrix 들의 크기를 통일할 수 있다는 장점이 있다.

## 4.2 Affine Transform

- Affine transform(아핀 변환)은 scaling(크기 조정),  rotation(회전)과 같은 선형 변환에 translation(평행 이동)을 추가하여 구성된 선형 변환의 특수한 형태이다.
- 아핀 변환은 다음과 같이 구성된다.
	* Linear transforn
		* Scaling ($S$)
		* Rotation ($R$)
		* etc.
	* Translation ($T$)
- 동차 좌표를 이용하면, 모든 변환은 2 차원에서는 $3 \times 3$ 행렬로, 3 차원에서는 $4 \times 4$ 행렬로..와 같이 크기가 동일한 행렬로 표현할 수 있다.
- 행렬에서 교환법칙이 성립하지 않듯이, $SRT, RST$와 같이 변환의 순서가 바뀌면 물체는 같은 위치로 이동하지 않는다는 것을 기억하자.

### 4.2.1 2D Affine Transform with Homogeneous Coordinates

- Scaling ($S$):

$$S = \begin{pmatrix}
s_{x} & 0 & 0 \\
0 & s_{y} & 0 \\
0 & 0 & 1 \\
\end{pmatrix}$$
- Rotation ($R$):
	회전하는 방향은 $CCW$이다.

$$R = \begin{pmatrix}
\cos\theta & -\sin\theta & 0 \\
\sin\theta & \cos\theta & 0 \\
0 & 0 & 1 \\
\end{pmatrix}$$
- Translation ($T$):

$$T = \begin{pmatrix}
1 & 0 & d_{x} \\
0 & 1 & d_{y} \\
0 & 0 & 1 \\
\end{pmatrix}$$

### 4.2.2 3D Affine Transform with Homogeneous Coordinates

- Scaling ($S$):

$$S = \begin{pmatrix}
s_{x} & 0 & 0 & 0 \\
0 & s_{y} & 0 & 0 \\
0 & 0 & s_{z} & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}$$
- Rotation ($R$):
	회전하는 방향은 $CCW$이다.

$$R_{x} = \begin{pmatrix}
1 & 0 & 0 & 0 \\
0 & \cos\theta & -\sin\theta & 0 \\
0 & \sin\theta & \cos\theta & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}$$

$$R_{y} = \begin{pmatrix}
\cos\theta & 0 & \sin\theta & 0 \\
0 & 1 & 0 & 0 \\
-\sin\theta & 0 & \cos\theta & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}$$


$$R_{z} = \begin{pmatrix}
\cos\theta & -\sin\theta & 0 & 0 \\
\sin\theta & \cos\theta & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}$$
- Translation ($T$):

$$T = \begin{pmatrix}
1 & 0 & 0 & d_{x} \\
0 & 1 & 0 & d_{y} \\
0 & 0 & 1 & d_{z} \\
0 & 0 & 0 & 1 \\
\end{pmatrix}$$

### 4.2.3 Concept of $[L|t]$

- $[L|t]$는 $[$ Affine Matrix $]$  $[$ Translation Vector $]$표기법으로, 아핀 변환 행렬의 표현 방식 중 하나이다.
- $[L|t]$는 아핀 변환 행렬에서 마지막 row를 제외한 나머지 부분이다.

$$\begin{pmatrix}
L_{11} & L_{12} & L_{13} & t_{14} \\
L_{21} & L_{22} & L_{23} & t_{24} \\
L_{31} & L_{32} & L_{33} & t_{34} \\
0 & 0 & 0 & 1
\end{pmatrix}  $$
- Affine Matrix ($L$):
	$L$은 $T$를 제외한 모든 선형 변환($S$, $R$ 등)이 합쳐진 것, 즉 combined linear transform을 뜻한다.
- Tanslation Vector ($t$):
	$t$는 선형 변환이 적용 된 $T$ 변형, 즉 conbined translation을 뜻한다.
 - 2D affine transform에서의 $[L|t]$:
	곱해지는 아핀 행렬의 개수나 종류에 상관 없이, 우리는 $[L|t]$라는 하나의 행렬을 얻는다.
	$[L|t]$가 점 $p$를 옮기는 방식은 $Lp+t$로 표현될 수 있다.
 - Rigid motion affine transform에서의 $[L|t]$:
	곱해지는 아핀 행렬의 개수나 종류에 상관 없이, 우리는 $[L|t]$라는 하나의 행렬을 얻는다.
	강체의 움직임에서, 우리는 $S$ 변형을 다루지 않는다. 강체는 늘어나거나 줄어들지 않기 때문이다.
	$[L|t]$가 점 $p$를 옮기는 방식은 $Rp+t$로 표현될 수 있다.

## 4.3 World Transform

- Object-space (OS):
	처음 object가 만들어질 때 썼던 좌표계를 object-space라고 한다.
	OS는 object에 붙어서 WS에서 object가 이동할 때 함께 이동한다.
	예를 들자면 Max에서 modeling을 하는 것이다.
- World-space (WS):
	Object가 배치되는 세계의 공간을 world-space라고 한다. 이 세계는 게임일수도, 애니메이션일수도 있다.
	예를 들자면 Unity에서 배치된 물체를 사용자 정의에 따라 다루는 것이다.
- World transform:
	OS에서 WS로 물체를 배치하는 것을 world transform이라고 한다.

## 4.4 Rotation and Object-space Basis

- Object space의 basis는 $\{u, v, n\}$이다.
- World space의 basis는 $\{e_1, e_2, e_3\}$이다.
- WS에서 물체가 돌아가면 물체와 같이 OS basis도 돌아가기 때문에, OS basis로 $R$ 행렬을 정의할 수 있다.
$$\{u, v, n\} \Longleftrightarrow R$$

$$R\begin{pmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{pmatrix}=
\begin{pmatrix}
u_x & v_x & n_x \\
u_y & v_y & n_y \\
u_z & v_z & n_z \\
\end{pmatrix}$$

## 4.5 Inverses of 3D Affine Transform

- 물체를 움직였을 때, 어떻게 움직여야 제자리로 돌아가는지 정의하는 행렬을 inverse matrix로 정의할 수 있다.
	이때, $A^{-1}A = I$이다.

### 4.5.1 Inverse of Scaling and Translation

- Inverse scaling:
$$S^{-1} \Longleftrightarrow S$$

$$\begin{pmatrix}
\frac{1}{s_{x}} & 0 & 0 & 0 \\
0 & \frac{1}{s_{y}} & 0 & 0 \\
0 & 0 & \frac{1}{s_{z}} & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}
\Longleftrightarrow
\begin{pmatrix}
s_{x} & 0 & 0 & 0 \\
0 & s_{y} & 0 & 0 \\
0 & 0 & s_{z} & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}
$$
- Inverse translation:
$$T^{-1} \Longleftrightarrow T$$

$$\begin{pmatrix}
1 & 0 & 0 & -d_{x} \\
0 & 1 & 0 & -d_{y} \\
0 & 0 & 1 & -d_{z} \\
0 & 0 & 0 & 1 \\
\end{pmatrix}
\Longleftrightarrow
\begin{pmatrix}
1 & 0 & 0 & d_{x} \\
0 & 1 & 0 & d_{y} \\
0 & 0 & 1 & d_{z} \\
0 & 0 & 0 & 1 \\
\end{pmatrix}
$$

### 4.5.2 Inverse of Rotation

- Inverse rotation:
	$4\times4$인 동차 좌표계에서 $R$을 표현하는 $3\times3$부분의 역수를 취하면 된다.
$$R^{-1} = R^{T} \Longleftrightarrow R$$

$$\begin{pmatrix}
u_x & u_y & u_z \\
v_x & v_y & v_z \\
n_x & n_y & n_z \\
\end{pmatrix}
\Longleftrightarrow
\begin{pmatrix}
u_x & v_x & n_x \\
u_y & v_y & n_y \\
u_z & v_z & n_z \\
\end{pmatrix}
$$

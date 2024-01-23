- OpenGL은 주로 C++로 작동되는 그래픽 **API(Application Programming Interface)** 이다.
	- 여기서 API란, 응용 프로그램이 다른 응용 프로그램과 상호 작용하기 위해 사용되는 인터페이스이다.
- 필요한 배경 지식은 C++, 선형대수학, 기하학, 삼각법 등이다. C++도 단순히 "Hello World"만 출력할수 있는 수준이 아니라 객체 지향과 관련된 구현을 꽤나 할 줄 알아야 한다. 수학적 지식은 그렇게 어려운 내용을 다루지는 않으니 안심해도 된다.
- OpenGL pdf에서
	- 초록색 글자: Definition
	- 빨간색 글자: Program structure
	- 파란색 글자: Variables

## 1. OpenGL

![](https://upload.wikimedia.org/wikipedia/commons/2/21/OpenGL_logo.svg)
- OpenGL이 뭔지 정확히 알아보자. OpenGL은 그래픽과 이미지들을 다룰 수 있는 폭 넓은 함수들을 제공해주는 API이다.
- OpenGL의 규격은 구현을 어떻게 해야 할 지 세부사항을을 딱히 주지 않는다. 그래서 실제 개발될 경우 사용자 정의에 따라 다양하게 구현될 수 있다.
- OpenGL이 그래픽 드라이버에 직접적으로 영향을 끼치는 API이다 보니 오류가 나면 다른 시도를 하는 것 보다 그래픽 드라이버를 업데이트 하는 것을 추천한다.

### 1.1. Core Profile vs Immediate Mode

- Immediate mode:
	- OpenGL의 초기 버전부터 사용되던 방식 중 하나로, 간단하게 코드를 작성할 수 있어 초보자들이 접근하기는 쉽지만, 성능 상의 이슈와 유지 보구의 어려움으로 인해 현대적인 OpenGL에서는 권장되지 않는다.
- Core profile:
	- OpenGL의 후기 버전부터 사용되던 방식 중 하나로, 고전적인 기능들을 제외하고 최신의 그래픽스 프로그래밍 기법을 지원하는 프로파일이다.
	- 더 현대적이고 효율적인 방식으로 OpenGL을 사용할 수 있도록 도와준다.
	- 셰이더 프로그래밍과 다른 다양한 현대적인 기술들을 사용할 수 있다.
- OpenGL의 가장 최신 기능을 사용하려면 역시 최신의 그래픽 카드가 필요하다. 그래서 개발자들은 호환성을 위해 예전 버전의 OpenGL로 개발하는 것을 선호하고는 한다.
### 1.2. Extensions

- OpenGL의 큰 특징 중 하나는 extension을 지원한다는 것이다. Extension을 이용하면 개발자들은 굳이 OpenGL 업데이트를 기다릴 필요 없이 그래픽 카드가 해당 extension을 지원하는지만 확인해도 원하는 그래픽이나 렌더링과 관련된 새로운 기능을 쓸 수 있다.
- 인기가 많은 extension은 정말로 다음 OpenGL의 업데이트에 포합되기도 한다. 옵시디언의 확장 프로그램 정도로 생각하면 이해하기 편할것이다.

### 1.3. State Machine

- OpenGL은 그 자체로 하나의 큰 state machine이다. **State machine**이라는 의미는, OpenGL이 그래픽 파이프 라인의 상태를 다양한 상태 변수들로 나타내고, 이를 변경하여 그래픽스 렌더링의 동작을 제어하는 방식을 의미한다.
- 예를 들어, 우리가 삼각형이 아닌 line으로 그래픽을 그리고 싶을 때, 단순히 삼각형이 아니라 line으로 그리라고 지시하는 것 만으로도 우리는 원하는 동작을 실행시킬 수 있다.
- OpenGL에는 다양한 상태 변수가 있는데, 그중 일부는 다음과 같다.
	1. **색깔 상태(Color State):** 현재 사용 중인 색깔이나 색상 관련 속성들을 나타낸다.
	2. **라이팅 상태(Lighting State):** 조명과 관련된 상태를 나타낸다.
	3. **뎁스 테스트 상태(Depth Test State):** 깊이 테스트와 관련된 상태를 나타낸다.
	4. **블렌딩 상태(Blending State):** 투명도와 관련된 상태를 나타낸다.

### 1.4. Objects

- OpenGL 라이브러리는 C언어로 작성되어 있다. C로 작성되어 있어 다른 언어로 파생해서 개발할 수 있다. 하지만 핵심은 여전히 C 라이브러리에 기반한다.
- C가 워낙 low-level의 언어다 보니 high-level의 언어로 해석 되기에는 어려움이 있어, 개발자들은 abstraction을 염두에 두고 OpenGL을 개발하였다. 이 abstraction이 바로 **object**들이다.
- 다음은 object를 다루는 OpenGL 코드의 예시이다.
```cpp
unsigned int objectId = 0;
glGenObject(1, &objectId);
// 객체 생성

glBindObject(GL_WINDOW_TARGET, objectId);
// 생성된 객체를 OpenGL context에 바인딩

glSetObjectOption(GL_WINDOW_TARGET, GL_OPTION_WINDOW_WIDTH, 800);
glSetObjectOption(GL_WINDOW_TARGET, GL_OPTION_WINDOW_HEIGHT, 600);
// GL_WINDOW_TARGET에 바인딩된 객체에 대한 옵션 결정

glBindObject(GL_WINDOW_TARGET, 0);
// 바인딩된 객체를 default로 설정(un-bind)
```
- Object는 우리가 만약에 모델을 만들 때, 여러 개의 객체를 사용하여 여러 모델을 지정할 수 있다. 모델링을 하기 위해 모든 옵션을 다시 설정하지 않아고 단순히 해당하는 객체들을 바인딩하는 것으로 충분하다는 뜻이다. `(아직 뭔 소린지 잘 모르겠다..)`

### 1.5. Let's get started

- 여기까지 OpenGL의 간단한 concept에 대해서 알아보았다. 여기까지 다 이해가 안됐다 하더라도 괜찮다. 이해가 안됐던 부분들은 뒤에서 차근 차근 추가 내용을 알아가며 습득할 수 있을 것이다.

## 2. Createing a Window

OpenGL로 삐까뻔쩍한 것들을 만들기 전에, 우리가 제일 먼저 해야되는 것은 그것들을 그리기 위한 **OpenGL context**와 **application window** 를 만드는 것이다. 따라서 우리는 윈도우를 만들고, context를 정의하고 user input을 다뤄야 한다.
이때 우리는 이와 관련된 함수를 사용하기 위해 다양한 라이브러리 **GLUT, SDL, SFML, GLFW** 등을 사용할 수 있다. 우리는 이 중 **GLFW**를 쓸 것이다.

### 2.1. GLFW

- **GLFW**는 OpenGL 개발자들을 위한 간단하고 효율적인 크로스 플랫폼 라이브러리이다. 주로 창을 생성하고 사용자 입력을 처리하는 데 사용된다.
- **GLFW**의 주요 특징은 다음과 같다:
	1. **창 관리(Window Management):** 창을 생성하고 관리하는 기능을 제공한다. 우리는 이를 통해 OpenGL context를 생성하고 창의 크기, 제목 등을 설정할 수 있다.
	2. **입력 처리(Input Handling):** 키보드, 마우스, 조이스틱 등의 입력 장치에 대한 이벤트 처리를 지원한다. 사용자의 입력에 따라 call-back 함수를 등록하여 원하는 동작을 수행할 수 있다.
	3. **시간 관리(Time Handling):** 시간 관리 기능을 통해 프레임 속도를 제한하거나 시간 간격에 따른 작업릉 수행할 수 있다.
	4. **스레드 지원(Thread Support):** 멀티스레드 환경에서 안전하게 사용할 수 있는 함수를 제공한다.
- **GLFW**는 주로 C언어로 작성되었지만, C++, Python, Java Rust 등에서도 사용할 수 있다.

### 2.2. Building GLFW

- GLFW는 여기서 다운 받을 수 있다. ([Download GLFW](https://www.glfw.org/download.html)) 일단 먼저 사이트에 들어가서 **Source package**를 클릭해 `glfw-3.3.9.zip`파일을 다운 받아주자.
- VS Studio에 이미 컴파일 된 바이너리 파일들이 기본적으로 깔려 있지만 우리는 완전성을 위해 GLFW의 소스 코드를 직접 컴파일 할 것이다. (그냥 바이너리 파일을 쓰지 않고 대체 왜...) 라고 말할 수도 있지만 소스 코드로부터 컴파일을 하는 과정은 결과물인 라이브러리가 나의 CPU/OS에 딱 맞게 돌아가도록 해준다.
- 모든 사람이 동일한 IDE나 빌드 시스템을 사용하지 않기 때문에 제공된 프로젝트/솔루션 파일이 다른 사람들의 설정과 호환되지 않을 수도 있다. 이 때문에 **CMake**라는 도구가 있다.

#### 2.2.1. CMake

- **CMake**는 사전 정의된 Cmake 스크립트를 기반으로 사용자가 사용하는 IDE/빌드 시스템(e.g. VS, Code::Blocks, Eclipse)에 맞춰 프로젝트/솔루션 파일을 생성할 수 있도록 도와주는 도구이다. 
- **CMake**는 여기서 다운 받을 수 있다. ([Download CMake](https://cmake.org/download/)) 필자는 OS에 맞춰 **Windows $\times$ 64ZIP**에 해당하는 파일을 다운 받았다.
- Cmake을 다운 했다면 폴더를 압축해제 한 후 bin 폴더 안에 있는 `cmake-gui.exe`를 실행시켜준다.
- `Where is the source code`에는 `GLFW`경로를 입력하고, `Where to build the binaries`에는 `GLFW경로/buid`를 입력한다.![Screenshot](./img/OpenGL_Cmake.png?raw=true)
- 하단의 `Configure`을 클릭하여 `build`디렉토리 생성 메세지가 나오면 `Yes`를 누르고, 아래 창이 뜨면 본인이 사용하고 있는 IDE 버전을 정확히 확인하고 `Finish`를 누른다. (본인은 Visual Studio 17 2022를 선택했다.)![](./img/OpenGL_Cmake_1.png?raw=true)
- 다시 `Configure`을 누르고 컴파일이 무사히 완료되면 `Generate`를 눌러 아까 만든 `build`폴더에 프로젝트 파일을 생성한다.
- 이제 `build`폴더에 들어가면 `GLFW.sln`파일이 무사히 생성된 것을 확인할 수 있다.<br>![](./img/OpenGL_Cmake_2.png?raw=true)

#### 2.2.2. Compile

- `GLWL.sln`파일을 Visual Studio로 열고, `Ctrl + F5`를 누른다.![](./img/OpenGL_Cmake_3.png?raw=true)
- 이렇게 파일을 실행시키고 나면, `GLFW 폴더/build/src/Debug`에 `glfw.lib`라이브러리 파일이 생긴 것을 확인할 수 있다.<br>![](./img/OpenGL_Cmake_4.png?raw=true)
- 여기까지 라이브러리 파일을 만드는 데에는 성공했다. 이제 우리는 OpenGL 프로그램에 쓸 GLFW 라이브러리의 파일과 include 파일이 어디 있는지만 IDE에게 확인시켜주면 된다.
- 컴퓨터의 적당한 곳에(예를 들어 로컬 폴더 C:) `OpenGL`폴더를 만들고 `lib`폴더도 만든 뒤 안에 `glfw.lib`파일을 넣고 `include`폴더를 복사 붙여넣기 해주면 OpenGL 프로젝트를 시작할 준비가 끝난 것이다.<br>![](./img/OpenGL_Compile_1.png?raw=true)

### 2.3. First Project

- 이제 Visual Studio를 열고, 언어로 C++을 선택하여 새 프로젝트를 생성하자. 우리는 모든 것을 64-bit 기반으로 할 것이기 때문에 디버그 설정이 $\times$ 86로 설정이 되어있다면 $\times$ 64로 바꿔주자.<br>![](./img/OpenGL_Compile.png?raw=true)

### 2.4. Linking

- 위에서 우리가 만든 프로젝트가 `GLFW`를 사용하게 만들기 위해서 우리는 프로젝트와 라이브러리를 `link`해야한다.
- 프로젝트 이름에서 `Alt + Enter` (속성)에 들어가 `Include Directories`를 다음과 같이 편집한다. 아까 만든 OpenGL 폴더의 `lib`폴더와 `include`폴더의 경로를 가져와 각각 넣어준다.<br>![](./img/OpenGL_Linking.png?raw=true)
- 안 끝났다.... `링커` > `입력`에 들어가 추가 종속성에 우리가 쓸 라이브러리의 이름인 `glfw3.lib`을 써주자....<br>`opengl32.lib`은 왜 쓰냐? 당신이 Window 사용자라면 Microsoft SDK에 포함돼서 Visual Studio를 깔 때 같이 딸려오는 라이브러리이기 때문에 추가해주는 것이다.![](./img/OpenGL_Linking_1.png?raw=true)
- 당신이 Linux 사용자라면 `-lglfw3 -lGL -lX11 -lpthread -lXrandr -lXi -ldl`등을 컴파일 할 때 링커 세팅에 추가해주면 된다.
- GLFW 라이브러리의 세팅이 끝났다면, 우리는 코드에 다음과 같이 헤더 파일을 추가할 수 있다.
```cpp
#include <GLFW\glfw3.h>
```
- 이제 우리는 GLFW의 설정 및 구성을 마쳤다.

### 2.5. GLAD

- 끝난 줄 알았지? 아니다.. OpenGL은 표준 또는 규격에 불과하기 때문에 특정 그래픽 카드가 지원하는 드라이버를 통해 구현된다. OpenGL의 다양한 버전에 대한 드라이버가 있기 때문에 대부분의 함수들의 위치는 컴파일 타임에 알 수 없고, 런타임에 쿼리(query)해야 한다. 이러한 함수들의 위치를 찾아내고, 나중에 사용하기 위해 해당 함수들을 함수 포인터에 저장하는 것은 개발자의 역할이다.
- 함수의 위치를 찾는 것은 운영 체제(OS)에 따라 다른데, Window에서의 예시를 들자면 다음과 같다.
```c
typedef void (*GL_GENBUFFERS) (GLsizei, GLuint*);
// define the function’s prototype

GL_GENBUFFERS glGenBuffers = (GL_GENBUFFERS)wglGetProcAddress("glGenBuffers");
// find the function and assign it to a function pointer

unsigned int buffer; glGenBuffers(1, &buffer);
// function can now be called as normal
```
- 위에 보이는 코드에서 알 수 있듯이 이 일련의 과정은 번거롭고, 복잡하다. 그래서 이러한 작업을 자동화하기 위한 오픈 소스 라이브러리인 **GLAD**가 있는 것이다.
- **GLAD**를 사용하면 필요한 OpenGL 함수에 대한 선언을 자동으로 생성하고, 함수 포인터 초기화 및 관련된 작업을 단순화할 수 있다.

#### 2.5.1 Setting up GLAD

- **GLAD**를 세팅하는 방법은 우리가 흔히 알고 있는 오픈 소스 라이브러리의 세팅 방법과 살짝 다른 점이 있다.
- **GLAD**는 사이트를 이용하여 우리가 어떤 버전의 OpenGL을 쓸 건지 입력하면 거기에 맞춰서 모든 관련된 함수를 다운 받을 수 있게 해준다.
- 일단 GLAD 사이트에 들어가자. ([glad.dav1d.de](https://glad.dav1d.de/)) 그다음 옵션을 다음과 같이 설정하고 Generate를 눌러 `glad.zip`파일을 다운 받아준다.![](./img/OpenGL_GLAD.png?raw=true)
- `glad.zip`폴더를 압축해제 하고, `include`에 들어있는 `glad`와 `KHR`파일을 복사한 뒤 앞서 만들었던 `OpenGL`폴더의 `include`폴더에 붙여넣기 한다.<br>![](./img/OpenGL_GLAD_1.png?raw=true)
- `src`폴더에 들어있는 `glad.c`파일은 프로젝트 폴더에 넣어준다.
- GLAD의 세팅이 끝났다면, 우리는 코드에 다음과 같이 헤더 파일을 추가할 수 있다.
```cpp
#include <glad\glad.h>
```

## 3. Hello Window

- 이제 부터는 코드 블록을 예시로 들며 설명하는 형태로 OpenGL에 대해서 설명하겠다.
- 코드 전문이 궁금하다면 src/OpenGL/ 디렉토리를 참고하자.

```cpp
#include <glad/glad.h>
#include <GLFW/glfw3.h>
#include <iostream>
```
	GLFW를 포함하기 전에 GLAD를 포함해야 한다는 것을 잊지 말자.
	GLFW 뿐만 아니라 OpenGL이 필요한 다른 헤더 파일을 추가할 때도 마찬가지다.

```cpp
int main()
{
	glfwInit();
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
```
	glfwInit을 통해서 GLFW를 initiate한다.
	
	이 다음 우리는 glfwWindowHint를 통해서 GLFW를 설정할 수 있다.
	glfwWindowHint(설정할 옵션(여러 개의 설정들이 enum화 되어있다.), 옵션의 값 설정)
		GLFW_CONTEXT_VERSION_MAJOR: 사용할 OpenGL 컨텍스트의 주 버전
		GLFW_CONTEXT_VERSION_MINOR: 사용할 OpenGL 컨텍스트의 부 버전
	예를 들어 OpenGL 3.3은 주 버전이 3이고 부 버전이 3인 버전이다.
	이 코드를 사용하면 프로그램은 OpenGL 3.3 Core Profile을 사용하는 컨텍스트를 생성하려한다.
	OpenGL 기능이나 특성을 사용하기 위해 필요한 최소한의 버전을 지정하는 것이다.
	이렇게 하면 사용자가 제대로된 OpenGL 버전을 갖고 있지 않으면 GLFW가 실행되지 않는다.
	
	CORE_PROFILE을 사용한다는 것은 불필요한 하위 호환 기능 없이
	OpenGL 기능의 더 작은 서브셋에 엑세스 한다는 것을 의미한다.
	* Mac OS에서는
		glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
		라는 코드를 추가해야 초기화 작업이 완료된다.
	
	+ OpenGL 버전 확인하는 방법
	https://kyoungwhankim.github.io/ko/blog/opengl_version/

```cpp
GLFWwindow* window = glfwCreateWindow(800, 600, "HelloWindow", NULL, NULL);
if (window == NULL)
{
	std::cout << "Failde to create GLFW window" << std::endl;
	glfwTerminate();
	return -1;
}
glfwMakeContextCurrent(window);
```
	glfwCreateWindow(윈도우 창 가로, 윈도우 창 세로, 창 이름, NULL, NULL);
	마지막 두 인수는 무시해도 된다.
	창이 제대로 만들어졌는지 확인하고, 다음 작업으로 넘어간다.
	
	glfwMakeContextCurrent로 윈도우의 컨텍스트를 현재 스레드의 메인 컨텍스트로 만들라고 명령한다.

### 3.1. GLAD

```cpp
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
{
	std::cout << "Failed to initialize GLAD" << std::endl;
	return -1;
}
```
	앞선 챕터에서 말한대로, GLAD는 OpenGL의 함수를 로드하는 데 사용된다.
	따라서 어느 OpenGL 함수를 부르기 전에 먼저 glfwGetProcAddress를 사용하여
	현재 컨텍스트의 함수 포인터를 가져와 초기화 한다.
	이렇게 하면 사용자의 OS에 맞는 OpenGL의 함수 포인터를 가져올 수 있다.

### 3.2. Viewport

```cpp
glViewport(0, 0, 800, 600);
glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);
```
	렌더링을 시작하기 전에 마지막으로 해야할 것이 있다.
	렌더링 할 차원, 즉 dimension을 glViewport 함수를 이용해 세팅하는 건데,
	데이터와 좌표를 창에 따라 어떻게 표시할 건지 정의할 수 있다.
	
	* glViewport 함수를 이용한 2D 좌표 -> 화면 좌표 변환
		glViewport 함수는 OpenGL에게 렌더링 결과를 표시할 영역을 알려준다.
		이 함수는 뷰포트를 설정하며, 뷰표트는 윈도우의 일부분으로 생각할 수 있다.
	
		OpenGL은 일반적으로 정규화된 디바이스 좌표(Normalized Device Coordinates, NDC)를 사용하여 작업한다.
		NDC 좌표는 X, Y, Z 축 각각 -1에서 1까지의 범위를 갖는다.
		예를 들어, (-0.5, 0.5)의 좌표가 있다고 치면, 화면에서는 (200, 450)으로 매핑 된다.
		예를 들어, X 좌표가 -1일 때, 이는 화면의 가장 왼쪽에 해당하며, X 좌표가 1일 때, 이는 화면의 가장 오른쪽에 해당한다.
		Y 좌표도 비슷한 방식으로 매핑된다.
	
	아무튼, 뷰 포트를 설정하고 나서, 우리가 창의 크기를 바꿀 수도 있기 때문에
	framebuffer_size_callback를 콜백 함수를 이용하여 창 사이즈가 바뀔 때 뷰포트도 바뀔 수 있도록 해준다.
		framebuffer_size_callback(윈도우 변수, 너비, 높이);
	
	* Retina Displays
		Retina 디스플레이는 고해상도 디스플레이로, 일반적인 디스플레이보다 픽셀 밀도가 높다.
		화면에 표시되는 픽셀 수가 일반 디스플레이보다 많으므로, 윈도우 크기를 나타내는 너비와 높이의 크기가 더 큰 값으로 설정된다.
	
	glfwSetFramebufferSizeCallback  함수를 사용하면 창의 크기가 변경될 때마다 또는 창이 처음에 표시될 때마다
	특정 작업을 수행하는 콜백 함수를 등록할 수 있으며,
	Retina 디스플레이에서는 초기 입력 값보다 큰 값이 전달될 수 있다는 점을 고려해야 한다.
	
	* Callback Functions
		우리가 만든 함수로 접근하도록 하는 콜백 함수가 많이 있다.
		예를 들어, 조이스틱 입력값 변화 콜백 함수라던가, 프로세스 에서 메세지 콜백 함수 같은 것들이 있다.
		우리는 콜백 함수를 창을 만든 이후 ~ render loop 초기화 까지 쓸 수 있다.

### 3.3. Ready your engines

```cpp
while (!glfwWindowShouldClose(window))
{
	processInput(window);

	glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
	glClear(GL_COLOR_BUFFER_BIT);

	glfwSwapBuffers(window);
	glfwPollEvents();
}
```
	* Render Loop
		창이 우리가 프로그램을 종료할 때 까지 연속적인 이미지를 계속 표시할 수 있도록
		우리는 while loop가 필요한데, 이를 render loop 이라고 한다.
		Render loop은 우리가 GLFW에게 멈추라고 말할 때 까지 계속 돌아간다.
	
	glfwWindowShouldClose
		GLFW가 중지 됐는지 체크하는 함수이다.
	glfwPollEvents
		이벤트 트리거 함수이다. (키보드 입려그 마우스 움직임 등)
		윈도우 상태를 업데이트 하고, 콜백 방식으로 사용할 수 있는 기능을 호출한다.
	glfwSwapBuffers
		이 함수는 더블 버퍼링(Double Buffering)을 사용하는 OpenGL 애플리케이션에서 사용된다.
		현재 화면에 표시되는 프론트 버퍼와 렌더링을 위해 사용되는 백 버퍼를 교체한다.
		렌더링 작업이 모두 완료된 후, 이 함수를 호출하면
		프론트 버퍼와 백 버퍼가 교체되어 렌더링 결과가 실제 화면에 표시된다.
		* Front Buffer & Back Buffer
			Front Buffer: 화면에 실제로 표시되는 버퍼로, 사용자가 직접 보는 화면의 픽셀 데이터가 저장되어 있다.
			Back Buffer: 렌더링 작업을 수행하는 버퍼로, 화면에 직접 나타나지 않는다.
		* Double Buffering
			화면에 출력되기 전에 렌더링 작업을 모두 완료한 후에 그림을 한 번에 표시하는 기술이다.
		* Color Buffer
			OpenGL에서 렌더링 작업 결과를 저장하는 데 사용되는 메모리 영역이다.
			GLFW 창의 각 픽셀에 대한 색상 값을 저장하는 2D 버퍼이다.
		이렇게 두 개의 버퍼를 사용하여 flickering을 방지한다.

### 3.4. One last thing

```cpp
glfwTerminate();
```
	Render loop을 종료하고 난 뒤 GLFW에 할당된 모든 요소를 clean한다.
	main 함수의 마지막에 return 0;와 함꼐 삽입하면 된다.

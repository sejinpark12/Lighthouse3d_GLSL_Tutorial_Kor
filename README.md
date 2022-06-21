> ## Lighthouse3d.com의 GLSL tutorial 번역입니다.

# GLSL Tutorial - Core

GLSL tutorial의 업데이트 버전입니다. 이 튜토리얼에서는 core 버전만 다룹니다. compatibility 버전을 원하신다면 [GLSL 1.2 tutorial](http://www.lighthouse3d.com/tutorials/glsl-12-tutorial/)로 이동하세요.

이 튜토리얼의 목적은 GLSL과 OpenGL을 완벽히 설명하여 OpenGL 스펙을 대체하는 것이 아닙니다. OpenGL [documentation](https://www.khronos.org/registry/OpenGL/index_gl.php) 페이지의 문서를 확인해 보세요. 해당 스펙들은 중요하지만, 처음 배우기에는 어렵습니다. 그러므로 이 튜토리얼을 학습하면서 GLSL에 가볍게 입문한다고 생각해 주세요.

항상 그렇듯이, 오류를 고치기 위해서는 여러분의 도움이 반드시 필요합니다. 튜토리얼에는 버그, 실수, 모호한 설명이 언제나 존재합니다. 틀린 부분이 있더라도, 부디 너그럽게 이해해 주세요 :-). 여러분의 피드백이 중요합니다.

튜토리얼의 목차는 그래픽스 파이프라인을 간단히 살펴보는 것으로 시작합니다. 모든 구성요소를 설명하고, 각각에 대해 GLSL과 관련된 내용을 소개합니다([**_그래픽스 파이프라인 색션_**](#그래픽스-파이프라인)). 그런 다음 OpenGL에서 셰이더를 로드, 컴파일, 링크하고 최종적으로 실행하는 절차에 대해 알아봅니다([**_OpenGL 설정 색션_**](#OpenGL-설정)).

셰이더는 C와 매우 유사한 문법으로 작성합니다. 그렇기 때문에, C를 알고 있다면 학습 곡선이 너무 가파르지 않을 것입니다. C와 약간 다른 부분에 초점을 맞춰 몇 가지 개념이 제시됩니다([**_GLSL 구문 색션_**](#GLSL-구문)).

그래픽스 파이프라인에는 입력이 있어야 하고([**_어플리케이션->셰이더 통신 색션_**](#어플리케이션-셰이더-통신)), 여러 셰이더들은 서로 통신을 해야 합니다([**_셰이더 간 통신 - 인터페이스 색션_**](#셰이더-간-통신---인터페이스)).
다음 두 색션에서 위 내용에 대해 다룹니다.

셰이더 예제를 다루기 전 마지막으로, 셰이더 작성에 깊은 관련이 있는 몇 가지 문제에 대해 이야기하고 넘어갈 것입니다. 바로 공간(spaces), 변환(transformations), 보간(interpolation)입니다([**_예제로 가기 전에 고려해야할 몇 가지 사항들 색션_**](#예제로-가기-전에-고려해야할-몇-가지-사항들)).

마지막으로 예제를 다룹니다. 추후에 예제가 추가될 것입니다 :-)([**_셰이더 예제 색션_**](#셰이더-예제))

자, 이제 GLSL과 OpenGL을 즐겨보세요!

## 목차

### 그래픽스 파이프라인

- [간략한 파이프라인 다이어그램](./tutorials/01_pipeline/01_pipeline.md)
- [버텍스 셰이더](./tutorials/02_vertex_shader/02_vertex_shader.md)
- [프리미티브 어셈블리](./tutorials/03_primitive_assembly/03_primitive_assembly.md)
- [테셀레이션 셰이더](./tutorials/04_tessellation/04_tessellation.md)
- [지오메트리 셰이더](./tutorials/05_geometry_shader/05_geometry_shader.md)
- 지오메트리 세이더 예제
- [래스터라이제이션과 보간](./tutorials/07_rasterization/07_rasterization.md)
- [프레그먼트 셰이더](./tutorials/08_fragment_shader/08_fragment_shader.md)

### OpenGL 설정

- [OpenGL 설정 서론](./tutorials/09_opengl_setup/09_opengl_setup.md)
- [셰이더 생성](./tutorials/10_creating_a_shader/10_creating_a_shader.md)
- [프로그램 생성](./tutorials/11_creating_a_program/11_creating_a_program.md)
- 설정 예제
- 문제해결: Infolog
- 마무리

### GLSL 구문

- 데이터 타입
- 선언과 함수
- 서브루틴

### 어플리케이션->셰이더 통신

- 서론
- Attribute 변수
- Uniform 변수
- Uniform 블록

### 셰이더 간 통신 - 인터페이스

- 셰이더 간 통신

### 예제로 가기 전에 고려해야할 몇 가지 사항들

- 공간과 행렬
- 보간 문제
- OpenGL 스켈레톤

### 셰이더 예제

- 정육면체
  - 튜토리얼의 첫 번째 예제입니다. 이 예제는 아주 간단하고 스크린에 지오메트리를 그리기 위한 기초만을 다룹니다. uniform 블록을 사용하고 프로젝션-뷰-모델 행렬을 사용하여 점이 어떻게 변환되는지 보여줍니다.
- 색이 입혀진 정육면체
  - 두 번째 예제에서는 색상을 설정하는 몇 가지 방법에 대해 이야기합니다. 버텍스 어트리뷰트로 색상을 탐색하고 uniform 변수로 색상을 설정합니다. 또한 디스플레이에 입력된 색상 또는 계산 값을 사용한 셰이더 디버깅에 대해 소개합니다.
- 라이팅
  - 컴퓨터 그래픽스에서 라이팅은 매우 중요합니다. 이 예제에서는 기초 이론과 단순한 로컬 라이팅의 구현을 다룹니다. 일반적으로 쓰이는 디렉셔널, 포인트, 스팟 라이팅 세 가지 타입의 라이팅을 고러드(Gouraud) 모델과 퐁(Phong) 셰이딩 모델을 사용하여 구현합니다.
- 서론
- 버텍스 당 디렉셔널 라이트 1
- 버텍스 당 디렉셔널 라이트 2
- 픽셀 당 디렉셔널 라이트
- 포인트 라이트
- 스팟 라이트
- 텍스쳐 좌표 다루기
  - 텍스쳐 좌표는 이미지를 표면 위에 매핑하는 방법을 정의하는데 주로 사용됩니다. 그러나 텍스쳐 좌표는 매핑을 수행하는데만 국한되지 않습니다. 텍스쳐 좌표 자체로 모델에 색상을 칠할 수 있습니다. 여기서 텍스쳐 좌표 자체를 사용하는 몇 가지 예제를 살펴보고 `mix`와 `smoothstep`과 같은 새로운 GLSL 함수에 대해 배웁니다.
- 이미지 텍스쳐링

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/

# Lighthouse3d_GLSL_Tutorial_Kor
Lighthouse3d.com의 GLSL 튜토리얼 번역입니다.

## GLSL Tutorial - Core

GLSL tutorial의 업데이트된 버전입니다. 여기서는 core 버전만을 다룹니다. compatibility 버전을 원하신다면 [GLSL 1.2 tutorial](http://www.lighthouse3d.com/tutorials/glsl-12-tutorial/)로 이동하세요.

이 튜토리얼은 GLSL과 OpenGL을 완벽히 설명하여 OpenGL 스펙을 대체하는 것을 목표로 두지 않습니다. OpenGL [documentation](https://www.khronos.org/registry/OpenGL/index_gl.php) 페이지의 문서를 확인해보세요. 해당 스펙들은 중요하지만, 처음 배우기에는 어렵습니다. 그러므로 이 튜토리얼로 GLSL에 가볍게 입문한다고 생각해주세요.

항상 그렇듯이, 오류를 고치기 위해서는 여러분의 도움이 반드시 필요합니다. 튜토리얼에는 버그, 실수, 모호한 설명이 언제나 존재합니다. 제가 틀린 부분이 있을 수 있지만, 부디 너그럽게 이해해주세요 :-). 여러분의 피드백이 중요합니다.

이 튜토리얼에서는 그래픽스 파이프라인의 내용을 간단하게 살펴보는 것으로 시작합니다. 모든 구성요소를 설명하고, 각각에 대해 GLSL에 관련된 것들을 소개합니다. 그런 다음 OpenGL로 가서 셰이더를 로드하고 컴파일하고 링크하고 마지막으로 실행하는 절차에 대해 알아봅니다.

셰이더는 C와 매우 유사한 문법으로 작성합니다. 따라서, C를 알고 있다면 학습 곡선이 너무 가파르지 않을 것입니다. 약간 다른 부분에 초점을 맞춰 몇 가지 개념이 제시됩니다.

그래픽스 파이프라인은 입력이 있어야 하고, 셰이더는 파이프라인과 서로 통신을 해야합니다.
다음 두 장에서 위 내용에 대해 다룹니다.

셰이더 예제를 다루기 전에 셰이더 작성과 매우 관련이 있는 몇 가지 문제에 대해 마지막으로 이야기 하고 넘어갈 것입니다. 바로 공간(spaces), 변환(transformations), 보간(interpolation)입니다.

마지막으로 예제를 다룹니다. 추후에 예제가 추가될 것입니다 :-)

자, 이제 GLSL과 OpenGL을 즐겨보세요!
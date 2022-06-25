# GLSL Tutorial - uniform 변수

| [목차](../../README.md) | 이전: [attribute 변수](../19_attribute_variables/19_attribute_variables.md) | 다음: uniform 블록 |
| :---------------------- | ----------------------------------------------------------------------------------------------------: | -----------------: |

 uniform 변수는 드로우 콜 동안 상수처럼 동작합니다. 어플리케이션이 uniform 변수를 그래픽스 파이프라인으로 공급합니다. 그리고 파이프라인의 모든 단계에서 접근할 수 있습니다. 즉, 변수를 선언했다면 모든 셰이더에서 모든 uniform 변수에 접근할 수 있습니다. 셰이더에서 uniform 변수는 읽기 전용입니다.

 uniform 변수는 블록으로 그룹화되거나 개별적으로 사용할 수 있습니다. 여기서는 개별적인 정의에 대해 살펴보고 다음 색션에서 uniform 블록에 대해 알아보겠습니다.

 셰이더 안에서 uniform 변수는 `uniform` 키워드로 정의합니다. 예를 들어 셰이더에서 myVar라는 이름의 vec4 타입 uniform 변수를 정의한다면

 ```glsl
 uniform vec4 myVar;
 ```

 셰이더에서 uniform 변수를 초기화할 수 있습니다. 예를 들어 vec4 타입의 uniform 변수를 초기화하는 것은 다음과 같습니다:

 ```glsl
 uniform vec4 myVar = {0.5, 0.2, 0.7, 1.0};
 ```

 셰이더 내부 초기화(In-Shader-Initialization)은 uniform 변수를 편리하게 설정할 수 있게하므로 매우 좋습니다. 셰이더 초기화에서 디폴트값을 설정할 수 있으며 uniform 변수에 다른 값이 필요할 경우에만 어플리케이션에서 값을 설정하면 됩니다.

 어플리케이션에서 uniform 변수를 설정하기 위해서는 먼저 location을 얻어야 합니다. 다음 함수로 구할 수 있습니다:

 ```cpp
 GLint glGetUniformLocation(GLuint program, const char *name);
 ```

 **파라미터:**
 - program: **링크된** 프로그램의 핸들
 - name: uniform 변수명

**반환값:**
- 변수의 location. 또는 활성화된 uniform 변수들 중에 찾고자 하는 uniform 변수가 없다면 -1

활성화된 uniform 변수는 단지 선언된 것이 아니라 셰이더에서 실제로 사용되는 변수입니다. 컴파일러는 코드 상에서 사용되지 않는 변수를 얼마든지 제거할 수 있습니다. 그러므로, uniform 변수가 셰이더에 선언되어도 사용되지 않는다면 위의 함수는 -1을 반환할 수 있습니다.

| [목차](../../README.md) | 이전: [attribute 변수](../19_attribute_variables/19_attribute_variables.md) | 다음: uniform 블록 |
| :---------------------- | ----------------------------------------------------------------------------------------------------: | -----------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-12-tutorial/uniform-variables/

# GLSL Tutorial - 어플리케이션->셰이더 통신 서론

| [목차](../../README.md) | 이전: [서브루틴](../17_subroutines/17_subroutines.md) | 다음: [attribute 변수](../19_attribute_variables/19_attribute_variables.md) |
| :---------------------- | ----------------------------------------------------: | --------------------------------------------------------------------------: |

셰이더 파이프라인은 입력으로 버텍스를 받습니다. 버텍스들은 하나 이상의 속성(attribute)을 가집니다. 또한 OpenGL은 버텍스와 관련되지 않지만 셰이더 효과를 계산하는데 필요한 변수의 정의를 허용합니다. 예를 들면 라이트의 위치를 이러한 변수로 정의 수 있습니다.

GLSL의 관점에서 변수는 두 종류가 있습니다:

- attributes
- uniforms

uniform 변수는 OpenGL 드로우 콜의 모든 버텍스에서 상수입니다. OpenGL 드로우 콜의 모든 버텍스에서 변하지 않는 전역 변수라고 생각할 수 있습니다. 이러한 변수의 예는 변환 행렬, 라이트 속성, 안개 설정, 중력, 속력 등이 있습니다.

uniform 변수를 블록으로 그룹화하여 더 효율적인 데이터 설정과 어플리케이션에서 셰이더로의 전송도 가능합니다.

[attribute](http://www.lighthouse3d.com/tutorials/glsl-tutorial/attribute-variables/)는 각 버텍스에 대해 고유한 값이며 일반적으로 버텍스마다 값이 다릅니다. 가장 분명한 attribute는 버텍스의 위치입니다. 텍스처 좌표와 법선 벡터 또한 일반적인 버텍스 attribute들입니다. 이것들은 버퍼에 작성되고 특정 셰이더 attribute에 바인딩됩니다.

| [목차](../../README.md) | 이전: [서브루틴](../17_subroutines/17_subroutines.md) | 다음: [attribute 변수](../19_attribute_variables/19_attribute_variables.md) |
| :---------------------- | ----------------------------------------------------: | --------------------------------------------------------------------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/communication-application-shader/

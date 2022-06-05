# GLSL Tutorial - 버텍스 셰이더

|[목차](../README.md)|이전 장: [간략한 파이프라인 다이어그램](../01_pipeline/01_pipeline.md)|다음 장: 프리미티브 어셈블리|
|:--|--:|--:|

![vertex_shader](../images/02_vertex_shader/02_vertex_shader.png)

한 번에 한 버텍스 씩, 각 버텍스에서 버텍스 셰이더가 실행됩니다. 셰이더는 그래픽 프리미티브를 구성하는 다른 버텍스에 대한 정보가 없습니다. 그리고 버텍스가 속한 프리미티브의 타입이 무엇인지도 모릅니다. 셰이더는 하나의 입력 버텍스 당 하나의 출력 버텍스를 내보냅니다.
각 버텍스는 사용자가 정의한 입력 속성(input attribute) 세트를 가집니다. 예를 들어, 위치, 법선 벡터, 텍스쳐 좌표와 같은 것들이 있습니다. 또한 버텍스 셰이더는 uniform 변수에 접근할 수 있습니다. uniform 변수는 드로우콜 동안 모든 버텍스에서 읽기 전용 전역 변수처럼 동작합니다.

사용자 정의 변수뿐만 아니라, GLSL에서 버텍스 속성(vertex attribute) 세트를 정의합니다:

```glsl
in int gl_VertexID;
in int gl_InstanceID;
```

`gl_VertexID`는 속성 배열(attribute array)의 버텍스 인덱스를 참조합니다.

인스턴스를 사용할 때 셰이더는 입력 버텍스 당 n번 실행되며, n값은 `glDraw*` OpenGL 명령에서 지정 인스턴스의 수입니다. gl_InstanceID 변수는 인스턴스의 인덱스를 담고 있습니다. 이 변수는 인스턴스를 사용하지 않는다면 항상 0입니다.

버텍스 셰이더는 입력 속성을 받고 버텍스를 출력하고 그 속성들을 계산합니다. 버텍스 셰이더에서 쓰기에 사용할 수 있는 고유 출력 속성은 다음과 정의되어 있습니다:

```glsl
out gl_PerVertex {
    vec4 gl_Position;
    float gl_PointSize;
    float gl_ClipDistance[];
};
```

이들 중 하나를 사용하는 것은 선택 사항이지만 버텍스 셰이더 이후의 고정 함수 단계에서는 `gl_Position`변수 사용을 가정합니다. 이 변수는 출력 버텍스 위치의 동차좌표를 저정합니다.

또한 셰이더는 버텍스 변수별 사용자 정의를 출력할 수 있습니다.

아래에 이 기능들을 사용한 간단한 예제가 있습니다:

```glsl
#version 410

layout (std140) uniform Matrices {
    mat4 projModeViewMatrix;
    mat3 normalMatrix;
};

in vec3 position;
in vec3 normal;
in vec2 texCoord;

out VertexData {
    vec2 texCoord;
    vec3 normal;
} VertexOut;

void main()
{
    VertexOut.texCoord = texCoord;
    VertexOut.normal = normalize(normalMatrix * normal);
    gl_Position = projModeViewMatrix * vec4(position, 1.0);
}
```

|[목차](../README.md)|이전 장: [간략한 파이프라인 다이어그램](../01_pipeline/01_pipeline.md)|다음 장: 프리미티브 어셈블리|
|:--|--:|--:|


## 출처
http://www.lighthouse3d.com/tutorials/glsl-tutorial/vertex-shader/


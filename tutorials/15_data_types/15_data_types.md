# GLSL Tutorial - 데이터 타입

| [목차](../../README.md) | 이전: 마무리 | 다음: 선언과 함수 |
| :---------------------- | -----------: | ----------------: |

GLSL에서 사용 가능한 단순 데이터 타입은 다음과 같습니다:

- float
- double
- bool
- int
- uint

이 데이터 타입들은 bool을 제외한 C의 기본 데이터 타입들과 동일합니다.

위의 각 데이터 타입에 대해 2, 3, 4개의 요소를 가진 벡터 타입도 사용 가능합니다. 다음과 같이 선언합니다:

- vec{2,3,4} float형 벡터. 예를 들어 vec2, vec3, vec4
- dvec{2,3,4} double형 벡터
- bvec{2,3,4} bool형 벡터
- ivec{2,3,4} integer형 벡터
- uvec{2,3,4} unsigned interger형 벡터

그래픽스에서 많이 사용되는 float형, double형에 대한 2x2, 3x3, 4x4 정방행렬(square matrices)이 제공됩니다. 다음과 같습니다:

- mat2, dmat2
- mat3, dmat3
- mat4, dmat4

또한 float형, double형에 대한 일반적인 형태의 비정방행렬(non-square matrices)도 제공됩니다:

- mat{2,3,4}x{2,3,4} 예를 들어 mat2x4. 첫 번째 숫자가 열, 두 번째 숫자가 행.
- dmat{2,3,4}x{2,3,4}

행렬의 열과 행 즉, 첫 번째와 두 번째 숫자가 같다면 정방행렬의 정의와 동일합니다.

텍스처 접근을 하기 위해 사용하는 특별한 타입 세트가 있습니다. 바로 sampler라는 데이터 타입입니다. sampler는 텍셀(texel)이라고도 불리는 텍스처 값에 접근하는데 필요합니다.

텍스처 샘플링을 위한 가장 일반적인 데이터 타입은 다음과 같습니다:

- sampler1D - 1D 텍스처용
- sampler2D - 2D 텍스처용
- sampler3D - 3D 텍스처용
- samplerCube - 큐브맵 텍스처용
- sampler2DShadow - 그림자맵용

아토믹 카운터(atomic counter)는 OpenGL 4의 새로운 기능입니다. 더 자세한 내용은 [Atomic Counter Tutorial](http://www.lighthouse3d.com/tutorials/opengl-atomic-counters/)에서 알아볼 수 있습니다.

GLSL에서 배열은 C와 동일한 문법으로 선언할 수 있습니다. 하지만 C와 다르게 배열을 선언과 동시에 초기화할 수는 없습니다. 배열의 원소에 접근하는 것은 C와 동일합니다.

GLSL에서 구조체 또한 사용할 수 있습니다. 문법은 C와 동일합니다.

```glsl
struct dirlight {
    vec3 direction;
    vec3 color;
}
```

**변수**

단순 변수 선언은 C와 매우 유사합니다. 변수를 선언할 때 초기화할 수도 있습니다.

```glsl
float a, b;     // 변수 2개(맞습니다. C와 같은 방식으로 주석을 작성합니다.)
int c = 2;      // c는 2로 초기화 됩니다.
bool d = true;  // d는 true입니다.
```

다른 타입의 변수 선언도 같은 패턴을 따릅니다. GLSL은 초기화와 타입 캐스팅을 위해 생성자에 크게 의존합니다. 그러나, 묵시적 타입 변환(implicit conversion)에 대해서 완화된 정책을 채택했습니다. 예를 들어 int에서 float로, 타입이 더 일반적인 타입으로 묵시적 변환될 수 있습니다.

```glsl
float b = 2;                    // 묵시적 타입 변환
int a = 2;
float c = float(a);             // 이것 또한 가능합니다. c는 2.0

vec3 f;                         // vec3 타입으로 f 선언
vec3 g = vec3(1.0, 2.0, 3.0);   // 변수 g를 선언하고 초기화
```

GLSL에서 다른 변수를 사용하여 변수를 초기화하는 것이 매우 유연합니다. 필요한 개수의 요소를 제공하기만 하면 됩니다. 다음 예제를 보겠습니다.

```glsl
vec2 a = vec2(1.0, 2.0);
vec2 b = vec2(3.0, 4.0);

vec4 c = vec4(a, b)         // c = vec4(1.0, 2.0, 3.0, 4.0);

vec2 g = vec2(1.0, 2.0);

float h = 3.0;

vec3 j = vec3(g, h);
```

행렬 또한 이 패턴을 따릅니다. 행렬에 대한 다양한 생성자가 있습니다. 예를 들어 행렬을 초기화하는 생성자는 다음과 같습니다:

```glsl
mat4 m = mat4(1.0)                  // 행렬의 대각 성분을 1.0로 초기화

vec2 a = vec2(1.0, 2.0);
vec2 b = vec2(3.0, 4.0);

mat2 n = mat2(a, b);                // 열 우선으로 행렬을 할당

mat2 k = mat2(1.0, 0.0, 1.0, 0.0);  // 모든 요소를 지정
```

구조체의 선언과 초기화는 다음과 같습니다:

```glsl
struct dirlight {   // 타입 정의
    vec3 direction;
    vec3 color;
};

dirlight d1;

dirlight d2 = dirlight(vec3(1.0, 1.0, 0.0), vec3(0.8, 0.8, 0.4));
```

| [목차](../../README.md) | 이전: 마무리 | 다음: 선언과 함수 |
| :---------------------- | -----------: | ----------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/data-types/

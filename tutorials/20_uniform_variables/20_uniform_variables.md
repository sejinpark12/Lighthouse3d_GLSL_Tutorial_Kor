# GLSL Tutorial - uniform 변수

| [목차](../../README.md) | 이전: [attribute 변수](../19_attribute_variables/19_attribute_variables.md) | 다음: uniform 블록 |
| :---------------------- | --------------------------------------------------------------------------: | -----------------: |

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

프로그램이 링크되고 코드 안에서 변수가 효과적으로 사용된다고 가정하면 반환값은 변수의 location입니다. 이 반환값으로 나중에 변수의 값을 설정하는데 사용할 수 있습니다. 변수의 값을 설정하기 위해서, OpenGL은 모든 데이터 타입을 커버하는 다양한 함수군(family of function)들과 여러가지 변수값 설정 방법을 제공합니다. 예를 들어, 위에서 정의한 `myVar` 변수를 보겠습니다. 링크된 프로그램의 핸들이 `p`이고 `vec4` 타입 값을 설정한다고 가정하면 다음과 같이 코드를 작성할 수 있습니다:

```cpp
GLint myLoc = glGetUniformLocation(p, "myVar");
glProgramUniform4f(p, myLoc, 1.0f, 2.0f, 2.5f, 2.7f);
```

또는 이렇게도 작성할 수 있습니다.

```cpp
float myFloats[4] = {1.0f, 2.0f, 2.5f, 2.7f};
GLint myLoc = glGetUniformLocation(p, "myVar");
glProgramUniform4fv(p, myLoc, 1, myFloats);
```

```cpp
void glProgramUniform4f(GLuint program, GLint location, GLfloat f1, ..., GLfloat f4);
void glProgramUniform4fv(GLuint program, GLint location, GLsizei count, const GLfloat *values);
```

**파라미터:**

- program: 링크된 프로그램의 핸들
- location: 프로그램 p에서 uniform 변수의 location
- f1...f4, values: uniform 변수에 설정하는 값
- count: 설정할 항목의 개수. 기본 데이터 타입의 경우는 항상 1입니다. 배열을 고려할 경우는 이 값은 설정할 배열의 원소의 수입니다.

OpenGL에는 GLSL의 모든 기본 데이터 타입과 미리 정의된 행렬에 대해서도 위와 같은 동작의 함수들이 존재합니다.

**배열**

셰이더에 배열 타입의 변수가 있는 상황을 생각해보겠습니다. uniform 배열의 선언은 다음과 같습니다:

```glsl
uniform vec4 color[2];
```

어플리케이션에서 변수를 배열로 볼 수 있고 배열의 모든 요소를 한번에 설정할 수 있습니다. 구조체를 제외한 모든 기본 타입에서 가능합니다. 아래의 코드를 보세요.

```cpp
float myColors[8] = {1.0f, 0.0f, 0.0f, 0.0f, 0.0f, 1.0f, 0.0f, 0.0f};
GLint myLoc = glGetUniformLocation(p, "color");
glProgramUniform4fv(p, myLoc, 2, myColors);
```

`glProgramUniform4fv` 함수의 세 번째 파라미터를 2로 설정한 것을 주목해주세요. 2개의 `vec4` 타입 배열 요소를 설정하기 때문입니다. 어플리케이션의 `myColors` 배열은 8개의 float 배열 요소가 필요합니다.

또다른 방법으로는 각 요소를 개별적으로 설정할 수 있습니다. 예를 들어, GLSL 배열 color의 두 번째 요소인 `color[1]`를 설정하고자 한다면 다음과 같은 코드를 작성할 수 있습니다:

```cpp
GLfloat aColor[4] = {0.0f, 1.0f, 1.0f, 0.0f};
myLoc = glGetUniformLocation(p, "color[1]");
glProgramUniform4fv(p, myLoc, 1, aColor);
```

각각의 기본 데이터 타입에 대한 glProgramUniform 함수가 있고 아래의 vec4에 대한 함수와 같이 두 종류의 함수가 제공됩니다:

```cpp
void glProgramUniform4f(GLuint program, GLint location, GLfloat v1, ..., GLfloat v4);
void glProgramUniform4fv(GLuint program, GLint location, GLsizei count, GLfloat *v);
```

**파라미터:**

- program: 링크된 프로그램의 핸들
- location: 변수의 location
- v1...v4: uniform 변수에 업데이트할 vec4 타입의 각 요소값
- count: 설정할 vec4 타입 요소의 수
- v: uniform 변수에 업데이트할 배열의 포인터

행렬의 경우에는 특별한 버전이 존재합니다. mat4의 예제가 아래에 있습니다:

```cpp
glProgramUniformMatrix4fv(GLuint program, GLint location, GLsizei count, GLboolean transpose, GLfloat *v);
```

**파라미터:**

- program: 링크된 프로그램의 핸들
- location: 변수의 location
- count: 설정할 vec4 타입 요수의 수
- transpose: true일 경우 행렬은 uniform 변수에 로드되기 전에 전치됩니다.
- v: uniform 변수에 업데이트할 배열의 포인터

**구조체**

GLSL에서 구조체를 정의하는 것은 C의 방법과 유사합니다. 예를 들어, 다음 코드 스니펫에서 vec4 타입 변수 2개를 맴버로 가지는 구조체를 선언하고 해당 구조체 타입으로 uniform 변수를 정의합니다.

```glsl
struct Colors{
    vec4 Color1;
    vec4 Color2;
};

uniform Colors myColors;
```

셰이더에어 구조체의 필드값을 사용하기 위해서는 C와 동일한 표기법을 사용합니다. 예를 들어, 위의 구조체 선언이 다음 main 함수 위에 있다고 가정한다면:

```glsl
void main()
{
    vec4 aColor = myColors.Color1 + myColors.Color2;
    ...
}
```

어플리케이션에서 이 변수들을 설정하는 경우에도 C와 동일한 표기법을 사용합니다. 구조체에 대한 location을 얻을 수 없기 때문에 구조체 전체의 값을 설정할 수 없습니다. 반드시 한 필드값 씩 설정해야 합니다. 아래와 같은 방법으로 코드를 작성합니다:

```cpp
float myFloats[8] = {0.0, 1.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0};

GLint myLoc = glGetUniformLocation(p, "myColors.Color1");
glProgramUniform4fv(p, myLoc, 1, myFloats);
myLoc = glGetUniformLocation(p, "myColors.Color2");
glProgramUniform4fv(p, myLoc, 1, &(myFloats[4]));
```

구조체의 배열을 다루는 것은 간단합니다. 다음과 같이 uniform 변수를 선언한다고 생각해보겠습니다:

```glsl
uniform Colors myColors[2];
```

셰이더를 다음과 같이 작성할 수 있습니다:

```glsl
void main()
{
    vec4 aColor = myColors[0].Color1 + myColors[1].Color2;
    ...
}
```

어플리케이션에서 변수들을 설정하는 방법도 동일합니다:

```cpp
GLint myLoc;
float myFloats[8] = {0.0, 1.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0};
float myOtherFloats[8] = {1.0, 1.0, 0.0, 0.0, 0.0, 1.0, 1.0, 0.0};

myLoc = glGetUniformLocation(p, "myColors[0].Color1");
glProgramUniform4fv(p, myLoc, 1, myFloats);
myLoc = glGetUniformLocation(p, "myColors[0].Color2");
glProgramUniform4fv(p, myLoc, 1, &(myFloats[4]));
myLoc = glGetUniformLocation(p, "myColors[1].Color1");
glProgramUniform4fv(p, myLoc, 1, myOtherFloats);
myLoc = glGetUniformLocation(p, "myColors[1].Color2");
glProgramUniform4fv(p, myLoc, 1, &(myOtherFloats[4]));
```

| [목차](../../README.md) | 이전: [attribute 변수](../19_attribute_variables/19_attribute_variables.md) | 다음: uniform 블록 |
| :---------------------- | --------------------------------------------------------------------------: | -----------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-12-tutorial/uniform-variables/

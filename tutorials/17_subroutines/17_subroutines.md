# GLSL Tutorial - 서브루틴

| [목차](../../README.md) | 이전: [구문과 함수](../16_statements_and_functions/16_statements_and_functions.md) | 다음: 어플리케이션->셰이더 통신 서론 |
| :---------------------- | ---------------------------------------------------------------------------------: | -----------------------------------: |

서브루틴(subroutine)은 프로그램을 다시 빌드하지 않고 셰이더 동작의 동적 구성(dynamic configuration)을 허용하는 OpenGL 메커니즘입니다.

넓은 범위 동작을 제공할 수 있는 셰이더를 구성하고 싶다고 가정합니다. 일반적인 시나리오는 라이팅입니다. 몇개의 라이팅 알고리즘을 제공하는 단일 셰이더를 작성할 수 있고 uniform 변수의 사용하여 매번 사용하는 알고리즘을 제어할 수 있습니다. 이런 설정의 이유는 성능과 관련되어 있습니다. 즉, 실시간으로 최소 사전 정의 수준의 FPS를 얻는 최적의 알고리즘을 선택할 수 있습니다.

서브루틴을 사용법을 설명하는 것이 목적이기 때문에 너무나도 기본적인 예제를 다룰 것입니다. 이 예제에서는 uniform 변수의 값에 따라서 빨간색 또는 파란색의 가능한 두 가지 색상에서 하나의 색상을 설정합니다. 셰이더는 다음과 같습니다:

```glsl
#version 400

layout (std140) uniform Matrices {
    mat4 pvm;
};

in vec4 position;

out vec4 color;

uniform int redBlueFlag;

void main() {
    if (redBlueFlag == 1)
        color = vec4(1.0, 0.0, 0.0, 1.0);
    else
        color = vec4(0.0, 0.0, 1.0, 1.0);

    gl_Position = pvm * position;
}
```

다음과 같이 색상을 설정하는 함수도 사용할 수 있습니다:

```glsl
#version 400

layout (std140) uniform Matrices {
    mat4 pvm;
};

in vec4 position;

out vec4 color;

uniform int redBlueFlag;

vec4 redColor() {
    return vec4(0.0, 0.0, 1.0, 1.0);
}

void main() {
    if (redBlueFlag == 1)
        color = redColor();
    else
        color = blueColor();

    gl_Position = pvm * position;
}
```

결과의 단순함을 봤을 때 서브루틴을 사용하는 것은 확실히 과합니다. 하지만 위에서 언급했다시피 이 예제의 목적은 서브루틴 구문을 소개하는 것입니다.

서브루틴으로 들어가 봅시다. 가지고 있는 각 색상 옵션에 대한 서브루틴을 정의할 것입니다. 먼저 서브루틴 시그니처를 정의해야 합니다. 이 경우에는 인자가 없고 반환 타입이 vec4인 함수를 가집니다. 그리고 정의된 시그니처와 같이 사용할 함수를 정의합니다.

```glsl
// 시그니처
subroutine vec4 colorRedBlue ();

// 옵션 1
subroutine (colorRedBlue) vec4 redColor() {
    return vec4(1.0, 0.0, 0.0, 1.0);
}

// 옵션 2
sobroutine (colorRedBlue) vec4 blueColor() {
    return vec4(0.0, 0.0, 1.0, 1.0);
}
```

시그니처를 정의할 때 이름을 `colorRedBlue`로 지정했고 이것이 서브루틴의 타입명입니다. 서브루틴을 작성할 때 subroutine 키워드를 사용하고 괄호 안의 타입명 `colorRedBlue`는 서브루틴의 타입을 나타냅니다. 이런 세부 사항 외에는 사용자 정의 함수와 동일합니다.

그리고 어떤 옵션을 사용할지 조작하는 특별한 uniform 변수가 추가로 필요합니다. 이 변수는 `subroutine uniform` 변수로 선언합니다. 나중에 보겠지만 이 변수는 어플리케이션 쪽에서 설정됩니다.

```glsl
subroutine uniform colorRedBlue myRedBlueSelection;
```

다음과 같이 main 함수를 작성합니다:

```glsl
void main() {
    color = myRedBlueSelection();
    gl_Position = pvm * position;
}
```

`if` 문이 사라진 것을 볼 수 있듯이, main 함수가 더 간단해졌습니다. 서브루틴을 사용하면 코드가 더 깔끔해집니다. 반면에 관리해야 할 항목이 하나 더 늘어나기 때문에 디버깅이 더 어려워질 수 있습니다.

여러 개의 서브루틴 타입을 가질 수 있으며 각 서브루틴에서 많은 함수를 정의할 수 있습니다. 사실, 시그니처가 호환되는 한, 다른 타입과 함께 사용할 수 있는 함수를 작성할 수 있습니다.

```glsl
subroutine vec4 colorRedBlue();
subroutine vec4 colorRed();

subroutine (colorRedBlue, colorRed) vec4 redColor() {
    return vec4(1.0, 0.0, 0.0, 1.0);
}
```

어플리케이션쪽에서 어떤 서브루틴을 사용할지 선택해야 합니다. 각 루틴은 인덱스에 할당되어 있으며 다음의 함수로 이 인덱스를 검색할 수 있습니다:

```cpp
GLuint glGetSubroutineIndex(GLuint program, GLenum shaderType, const GLchar *name);
```

**파라미터:**

- `program`: 프로그램의 이름
- `shaderType`: 루틴이 정의된 셰이더의 단계. 반드시 `GL_VERTEX_SHADER`, `GL_TESS_CONTROL_SHADER`, `GL_TESS_EVALUATION_SHADER`, `GL_GEOMETRY_SHADER`, `GL_FRAGMENT_SHADER` 중 하나
- `name`: 서브루틴의 이름

다음 함수로 subroutine uniform location을 구할 수 있습니다.

```cpp
GLint glGetSubroutineUniformLocation(GLuint program, GLenum shaderType, const GLchar *name);
```

**파라미터:**

- `program`: 프로그램의 이름
- `shaderType`: 루틴이 정의된 셰이더의 단계. 반드시 `GL_VERTEX_SHADER`, `GL_TESS_CONTROL_SHADER`, `GL_TESS_EVALUATION_SHADER`, `GL_GEOMETRY_SHADER`, `GL_FRAGMENT_SHADER` 중 하나
- `name`: 서브루틴의 이름

예제로 다시 돌아와서, 다음과 같이 코드를 작성합니다:

```cpp
GLuint routineC1 = glGetSubroutineIndex(p, GL_VERTEX_SHADER, "redColor");
GLuint routineC2 = glGetSubroutineIndex(p, GL_VERTEX_SHADER, "blueColor");
GLuint v1 = glGetSubroutineUniformLocation(shader.getProgramIndex(), GL_VERTEX_SHADER, "myRedBlueSelection");
```

마지막으로 각 서브루틴 타입에 할당된 서브루틴을 설정해야 합니다. 여러 타입을 가질 수 있기 때문에, 배열의 인덱스가 subroutine uniform location이고 원소값이 서브루틴인 배열이 필요합니다. 이 연결을 설정하는 함수는 다음과 같습니다:

```cpp
void glUniformSubroutinesuiv(GLenum shaderType, GLsizei count, const GLuint *indices);
```

**파라미터:**

- `shaderType`: 루틴이 정의된 셰이더의 단계. 반드시 `GL_VERTEX_SHADER`, `GL_TESS_CONTROL_SHADER`, `GL_TESS_EVALUATION_SHADER`, `GL_GEOMETRY_SHADER`, `GL_FRAGMENT_SHADER` 중 하나
- `count`: 배열의 원소 개수
- `name`: 배열의 인덱스가 subroutine uniform location인 배열

위의 함수가 호출되기 전에 프로그램은 반드시 사용 중이어야 합니다. 게다가 이 연결은 프로그램 상태의 일부가 아닙니다. 그래서 프로그램이 다시 링크되거나 `glUseProgram`, `glBindProgramPipeline`, `glUseProgramStages`가 호출되는 즉시 사라집니다.

실제로는, `glUseProgram`과 드로잉 명령 사이에서 `glUniformSubroutinesuiv`를 호출해야 한다는 것을 의미합니다.

아래의 OpenGL 코드 스니펫은 subroutine uniform과 호환되는 서브루틴의 정보를 검색하는 방법을 보여줍니다. OpenGL에서 서브루틴에 대한 정보를 얻는 몇가지 함수를 보여줍니다.

```cpp
int maxSub, maxSubU, activeS, countActiveSU;
char name[256]; int len, numCompS;

glGetIntegerv(GL_MAX_SUBROUTINES, &maxSub);
glGetIntegerv(GL_MAX_SUBROUTINE_UNIFORM_LOCATIONS, &maxSubU);
printf("Max Subroutines: %d Max Subroutine Uniforms: %d\n", maxSub, maxSubU);

glGetProgramStageiv(p, GL_VERTEX_SHADER, GL_ACTIVE_SUBROUTINE_UNIFORMS, &countActiveSU);

for (int i = 0; i < countActiveSU; ++i) {
    glGetActiveSubroutineUniformName(p, GL_VERTEX_SHADER, i, 256, &len, name);

    printf("Subroutine Uniform: %d name: %s\n", i, name);
    glGetActiveSubroutineUniformiv(p, GL_VERTEX_SHADER, i, GL_NUM_COMPATIBLE_SUBROUTINES, &numCompS);

    int *s = (int*)malloc(sizeof(int) * numCompS);
    glGetActiveSubroutineUniformiv(p, GL_VERTEX_SHADER, i, GL_COMPATIBLE_SUBROUTINES, s);
    printf("Compatible Subroutines:\n");
    for (int j = 0; i < numComps; ++j) {
        glGetActiveSubroutineName(p, GL_VERTEX_SHADER, s[j], 256, &len, name);
        printf("\t%d - %s\n", s[j], name);
    }
    printf("\n");
    free(s);
}
```

위 코드는 다음과 같은 결과를 출력합니다:

```
Max Subroutines: 1024 Max Subroutine Uniforms: 1024
Subroutine Uniform: 0 name: myGreenSelection
Compatible Subroutines:
    2 - doNotUseGreen
    3 - useGreen

Subroutine Uniform: 1 name: myIntensitySelection
Compatible Subroutines:
    4 - halfIntensity
    5 - fullIntensity

Subroutine Uniform: 2 name: myRedBlueSelection
Compatible Subroutines:
    0 - blueColor
    1 - redColor
```

| [목차](../../README.md) | 이전: [구문과 함수](../16_statements_and_functions/16_statements_and_functions.md) | 다음: 어플리케이션->셰이더 통신 서론 |
| :---------------------- | ---------------------------------------------------------------------------------: | -----------------------------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/subroutines/

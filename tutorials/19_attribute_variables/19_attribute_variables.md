# GLSL Tutorial - attribute 변수

| [목차](../../README.md) | 이전: [어플리케이션->셰이더 통신 서론](../18_communication_app_shader/18_communication_app_shader.md) | 다음: uniform 변수 |
| :---------------------- | ----------------------------------------------------------------------------------------------------: | -----------------: |

이전 색션에서 말했던대로, 버텍스는 그래픽스 파이프라인에 공급되는 attribute를 가집니다. 오브젝트를 그리기 위해서는 오브젝트의 버텍스들을 지정해야 하고 면을 정의하기 위해 버텍스가 연결되는 방법을 지정해야 합니다. 또한 버텍스는 위치, 법선 벡터, 텍스처 좌표를 attribute로 가집니다.

버텍스 데이터를 셰이더로 보내기 위해서 버텍스 셰이더에서 attribute의 location을 알아야 합니다. OpenGL에서는 각 attribute에 대한 특정 location를 정의할 수 있습니다. 다른 방법으로 OpenGL에서 location을 정의하고 이후에 location의 값을 쿼리하는 방법이 있습니다.

attribute에 대한 location을 지정하기 위해서는 그전에 반드시 프로그램을 링크해야 합니다. 또는 프로그램이 이미 링크되어 있다면 이후에 다시 링크해야 합니다.

다음 버텍스 셰이더에서 발췌한 부분을 보세요:

```glsl
#version 330

in vec3 position;
in vec3 normal;
in vec3 texCoord;
...
```

`glBindAttribLocation` 함수는 attribute에 location을 바인딩하는데 사용할 수 있습니다.

```cpp
void glBindAttribLocation(GLuint program, GLuint index, const GLchar *name);
```

**파라미터:**

- program: 프로그램 오브젝트 핸들
- index: attribute에 바인딩할 인덱스
- name: 버텍스 attribute의 이름

위의 셰이더 코드를 사용하고 프로그램 오브젝트 핸들이 p라고 가정했을 때, 다음과 같이 attribute "position"을 location 0으로 바인딩할 수 있습니다:

```cpp
glBindAttribLocation(p, 0, "position");
```

location을 attribute에 바인딩한 후에 프로그램을 링크하는 것을 잊지마세요.

다른 방법은 OpenGL에서 적절한 location을 선택하게 하고 프로그램이 링크된 후에 쿼리하는 것입니다. 쿼리 함수는 `glGetAttribLocation`입니다.

```cpp
GLint glGetAttribLocation(GLuint program, const GLchar *name);
```

**파리미터:**

- program: 프로그램 오브젝트 핸들
- name: 버텍스 attribute의 이름

**반환값:**

- attribute에 바인딩된 location

예제: 프로그램 오브젝트 핸들이 p라고 가정합니다.

```cpp
GLint vertexLoc;

glLinkProgram(p);
vertexLoc = glGetAttribLocation(p, "position");
```

위의 코드가 실행되면 `vertexLoc`에 "position" attribute의 location이 저장됩니다.

이제 attribute가 _어디에_ 있는지 알고 있고, 배열을 정의하여 attribute의 값들이 어떻게 그래픽스 파이프라인으로 전송되는지 알아볼 것입니다.

attribute들을 정의하기 위해서는 먼저 배열에 데이터를 저장합니다. attribute들을 하나의 배열에 끼워 넣을 수도 있지만 여기서는 각 attribute에 대한 개별적인 배열을 생성할 것입니다.

```cpp
// 삼각형 세트의 데이터
float pos[] = {-1.0f, 0.0f, -5.0f, 1.0f,
                1.0f, 0.0f, -5.0f, 1.0f,
                0.0f, 2.0f, -5.0f, 1.0f, ...};

float textureCoord[] = { ... };

float normal[] = { ... };
```

모든 배열에 대한 인덱스 i를 고려하여 버텍스 i에 대한 attribute를 얻습니다.

인덱스 배열을 정의하면 버텍스를 자유롭게 연결하여 프리미티브를 생성할 수 있습니다. 그러므로 인덱스 배열을 다음과 같이 추가합니다:

```cpp
unsigned integer index[] = {0, 1, 2, ...};
```

인덱스 배열이 없다면 셰이더는 버텍스들을 순차적으로 처리할 것입니다.

각 attribute는 OpenGL 버퍼에 저장됩니다. 이 버퍼 세트는 OpenGL **Vertex Array Object** 또는 VAO의 일부분입니다.

우선 VAO를 생성하고 바인딩합니다:

```cpp
GLuint vao;
glGenVertexArrays(1, &vao);
glBindVertexArray(vao);
```

각각의 개별적인 attribute에 대해서, 다음의 과정을 진행합니다: 먼저 버퍼를 생성하고 바인딩하고 데이터를 넣고 마지막으로 그래픽스 파이프라인의 입력 attribute와 연결합니다.

예를 들어, pos 배열과 "position" attribute를 가정하면 다음과 같습니다:

```cpp
GLuint buffer;
glGenBuffers(1, &buffer);

// 프로그램 p에서 "position" attribute의 location을 구합니다.
vertexLoc = glGetAttribLocation(p, "position");

// position에 대한 버퍼를 바인딩합니다.
// attribute를 공급하는데 사용하는 버퍼의 타입은 GL_ARRAY_BUFFER입니다.
glBindBuffer(GL_ARRAY_BUFFER, buffer);

// buffer를 공급합니다. 그리고 OpenGL에게 버퍼의 데이터를 변경할 계획이 없으며(STATIC) 드로잉(DRAW)에 사용할 것임을 알립니다.
glBufferData(GL_ARRAY_BUFFER, sizeof(pos), pos, GL_STATIC_DRAW);

// 해당 location에서 attribute를사용합니다.
glEnableVertexAttribArray(vertexLoc);

// OpenGL에게 배열에 담긴 데이터의 정보를 알립니다:
// 각 버텍스에 대한 4개의 float 타입값이 들어있습니다.
glVertexAttribPointer(vertexLoc, 4, GL_FLOAT, 0, 0, 0);
```

다른 배열들도 위와 비슷한 방법으로 설정합니다. `normal` 배열은 버텍스 당 3개의 float 값을 가지고 `textureCoord` 배열은 2개의 값을 가진다는 것만 다릅니다.

인덱스 배열의 경우, 접근이 더 간단하며 주요 차이점은 버퍼의 타입입니다:

```cpp
GLuint buffer;
glGenBuffers(1, &buffer);

// 위치에 대한 버퍼를 바인딩합니다.
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, buffer);
// 버퍼에 데이터를 복사합니다.
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(index), index, GL_STATIC_DRAW);
```

렌더링 함수에서 OpenGL에게 위의 정의된 지오메트리를 그릴 것을 요청합니다. 이러한 목적으로 인덱스 버퍼를 포함하는 VAO를 다음과 같이 사용할 수 있습니다: 

```cpp
glUseProgram(p);
glBindVertexArray(vao);
glDrawElements(GL_TRIANGLES, index.size(), GL_UNSIGNED_INT, NULL);
```

또는, 인덱스가 없는 VAO의 경우는:

```cpp
glUseProgram(p);
glBindVertexArray(vao);
glDrawArrays(GL_TRIANGLES, 0, count);
```

| [목차](../../README.md) | 이전: [어플리케이션->셰이더 통신 서론](../18_communication_app_shader/18_communication_app_shader.md) | 다음: uniform 변수 |
| :---------------------- | ----------------------------------------------------------------------------------------------------: | -----------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/attribute-variables/

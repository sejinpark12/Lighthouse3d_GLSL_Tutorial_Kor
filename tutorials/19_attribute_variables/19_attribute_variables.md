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

| [목차](../../README.md) | 이전: [어플리케이션->셰이더 통신 서론](../18_communication_app_shader/18_communication_app_shader.md) | 다음: uniform 변수 |
| :---------------------- | ----------------------------------------------------------------------------------------------------: | -----------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/attribute-variables/

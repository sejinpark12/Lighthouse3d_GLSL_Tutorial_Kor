# GLSL Tutorial - 구문과 함수

| [목차](../../README.md) | 이전: [데이터 타입](../15_data_types/15_data_types.md) | 다음: [서브루틴](../17_subroutines/17_subroutines.md) |
| :---------------------- | -----------------------------------------------------: | ----------------------------------------------------: |

**조작흐름 구문(Control Flow Statements)**

사용할 수 있는 구문은 C와 매우 유사합니다. if-else와 같은 조건문, for, while, do-while문과 같은 반복문이 존재합니다.

```glsl
if (bool expression)
    ...
else
    ...

for (initialization; bool expression; loop expression)
    ...

while (bool expression)
    ...

do
    ...
while (bool expression)
```

점프하는 몇가지 키워드가 정의되어 있습니다:

- `continue` : 반복문에서 사용할 수 있고 반복문의 다음 반복으로 점프합니다.
- `break` : 반복문에서 사용할 수 있고 반복문을 종료합니다.
- `discard`

`discard` 키워드는 프레그먼트 셰이더에서만 사용할 수 있습니다. 프레임 버퍼나 깊이에 작성하지 않고 현재 프레그먼트에 대한 셰이더를 종료합니다.

**함수**

C와 마찬가지로 셰이더에서 함수를 만들 수 있습니다. 각 셰이더 타입은 반드시 다음과 같은 한 개의 main 함수를 가집니다:

```glsl
void main()
```

사용자 정의 함수를 작성할 수 있습니다. C와 같이 함수는 반환값을 가질 수 있고 반환문을 사용하여 함수의 결과를 전달해야 합니다. GLSL의 함수는 파라미터를 함수의 출력으로 선언할 수도 있습니다. 반환 타입은 배열을 제외한 모든 [타입](http://www.lighthouse3d.com/tutorials/glsl-tutorial/data-types/)일 수 있습니다.

함수의 파라미터는 다음 한정자를 가질 수 있습니다:

- `in` : 입력 파라미터
- `out` : 함수의 출력. 반환문은 함수의 결과를 보내기 위한 옵션이기도 합니다.
- `input` : 함수의 입출력 모두에 대한 파라미터

한정자가 지정되지 않으면 디폴트로 `in`으로 간주됩니다.

마지막으로 몇 가지 주의할 점:

- 함수는 파라미터 목록이 다른 경우에 오버로딩 될 수 있습니다.
- 재귀는 스펙에서 정의되지 않습니다.

이 세부 색션에 대한 내용의 함수 예제입니다.

```glsl
vec4 toonify(in float intensity) {
    vec4 color;
    if (intensity > 0.98)
        color = vec4(0.8, 0.8, 0.8, 1.0);
    else (intensity > 0.5)
        color = vec4(0.4, 0.4, 0.8, 1.0);
    else (intensity > 0.25)
        color = vec4(0.2, 0.2, 0.4, 1.0);
    else
        color = vec4(0.1, 0.1, 0.1, 1.0);

    return (color);
}
```

| [목차](../../README.md) | 이전: [데이터 타입](../15_data_types/15_data_types.md) | 다음: [서브루틴](../17_subroutines/17_subroutines.md) |
| :---------------------- | -----------------------------------------------------: | ----------------------------------------------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/statements-and-functions/

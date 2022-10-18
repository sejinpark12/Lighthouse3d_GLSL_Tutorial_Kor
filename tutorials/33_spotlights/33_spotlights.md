# GLSL Tutorial - 스팟 라이트

| [목차](../../README.md) | 이전: [포인트 라이트](./../32_point_lights/32_point_lights.md) | 다음: [텍스처 좌표 다루기](./../34_texture_coordinates/34_texture_coordinates.md)|
| :---------------------- | -------------------: | --------------: |

스팟 라이트는 제한된 [포인트 라이트](http://www.lighthouse3d.com/tutorials/glsl-tutorial/point-lights/)입니다. 즉, 스팟 라이트의 광선은 제한된 범위의 방향으로만 발산합니다. 이러한 제한을 정의하기 위해서는 일반적으로 원뿔을 사용합니다만 다른 도형도 가능합니다.

다음 그림은 원뿔 모양의 스팟 라이트와 관련 데이터를 보여줍니다.

<p align="center"><img src="../../images/33_spotlights/spotLightDiagram.png"  width="250"></p>

- position: 원뿔의 꼭대기(sp)
- spot direction: 원뿔 축의 방향을 정의하는 벡터(sd)
- cutoff angle: 원뿔의 조리개. 각도가 방향 벡터에서 원뿔의 경계까지 측정된다고 가정($\alpha$)

버텍스 셰이더는 [포인트 라이트](http://www.lighthouse3d.com/tutorials/glsl-tutorial/point-lights/)와 거의 동일합니다. 

| [목차](../../README.md) | 이전: [포인트 라이트](./../32_point_lights/32_point_lights.md) | 다음: [텍스처 좌표 다루기](./../34_texture_coordinates/34_texture_coordinates.md)|
| :---------------------- | -------------------: | --------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/spotlights/
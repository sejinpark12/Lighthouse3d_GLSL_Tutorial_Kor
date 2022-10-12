# GLSL Tutorial - 이미지 텍스처링

| [목차](../../README.md) | 이전: [텍스처 좌표 다루기](../34_texture_coordinates/34_texture_coordinates.md) | 다음:  |
| :---------------------- | -------------------: | --------------: |

대체로 텍스처는 폴리곤 메시에 입히려고 하는 이미지입니다. 마치 벽에 벽지를 붙히는 것과 같습니다. 때때로 텍스처는 색상이 아닌 데이터로 볼 수 있습니다. 텍스처 좌표는 이미지와 폴리곤 메시 사이의 매핑에 사용합니다. 어플리케이션 내부에서 텍스처를 사용하려면 셰이더에서 실제로 텍스처를 사용하기 전에 OpenGL에서 몇 가지 설정을 해야합니다. 이 섹션에서는 몇 가지 대표적인 텍스처 사용법을 다룰 것입니다.

**OpenGL**

셰이더에서 텍스처를 사용하려면 우선 OpenGL 텍스처 오브젝트를 생성해야 합니다. 그리고 일반적으로 파일에서 이미지를 로드하여 텍스처에 데이터를 저장해야 합니다. 어떻게 해야하는지 모르겠다면 [이 페이지](http://www.lighthouse3d.com/cg-topics/code-samples/loading-an-image-and-creating-a-texture/)를 보세요.

셰이더 설정은 텍스처 유닛을 지정하는 uniform이 필요합니다. 셰이더가 텍스처 네임이 아닌 텍스처 유닛을 받는다는 것에 주의하세요.

**GLSL**

**Retrieving the texture data**

**NOTE**

| [목차](../../README.md) | 이전: [텍스처 좌표 다루기](../34_texture_coordinates/34_texture_coordinates.md) | 다음:  |
| :---------------------- | -------------------: | --------------: |

## 출처

http://www.lighthouse3d.com/tutorials/glsl-tutorial/texturing-with-images/

---
layout: single

title: "[UE5] 언리얼 엔진 메인 머티리얼 노드"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-12-19
last_modified_at: 2024-12-19

order : 150
---

# 메인 머티리얼 노드

메인 머티리얼 노드는 모든 머티리얼 네트워크가 종료되는 지점이며, 출력을 담당합니다.

메인 머티리얼 노드 입력에 어떤 머티리얼 표현식 노드 조합이 연결되는지에 따라, 최종 머티리얼의 외관과 성능을 결정합니다.

## 머티리얼 입력 예시

![MainMaterialNode-InputExample]({{site.url}}/images/Unreal/ue5/2024-12-18-MainMaterialNode/MainMaterialNode-InputExample.PNG)

그래프의 머티리얼 표현식에서 데이터를 전혀 받지 않는 입력은 단순히 디폴트값으로 되돌아갑니다.

예를 들어 메탈릭(Metallic), 스페큘러(Specular), 러프니스(Roughness)에 연결된 것이 없더라도, 여전히 머티리얼의 외관에 영향을 미칩니다.

## 활성화 및 비활성화된 입력

메인 머티리얼 노드의 일부 입력 핀은 활성화되어 있지만, 기본적으로 비활성화된 입력 핀도 있습니다.  
아래에 디테일 패널에 있는 프로퍼티가 어떤 입력이 활성화될지 결정합니다.

+ 머티리얼 도메인(Material Domain)
+ 블렌드 모드(Blend Mode)
+ 셰이딩 모델(Shading Model)

![MainMaterialNode-NodeVariations]({{site.url}}/images/Unreal/ue5/2024-12-18-MainMaterialNode/MainMaterialNode-NodeVariations.PNG)

비활성화된 입력에 노드가 연결되면, 연결된 노드는 무시됩니다.  
즉, 어떤 방식으로도 컴파일된 머티리얼에 영향을 미치지 않습니다.

디테일 패널의 프로퍼티에 대해 자세한 설명은 아래 링크로 알 수 있습니다.

[언리얼 엔진 머티리얼 프로퍼티](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/unreal-engine-material-properties){: target="_blank"}

## 입력 설명

모든 머티리얼에 모든 입력이 전부 필요하지는 않습니다.

아래 링크에 메인 머티리얼 노드의 모든 입력에 대한 설명과 예시가 이미지로 설명되어있습니다.

[언리얼 엔진 머티리얼 입력](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/material-inputs-in-unreal-engine){: target="_blank"}
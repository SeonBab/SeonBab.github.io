---
layout: single

title: "[수학] 좌표계"

categories:
    - Math
tag: [수학]

date: 2025-02-14
last_modified_at: 2025-02-14

order : 150
---

# 좌표계

게임에서 좌표계는 매우 중요한 개념입니다.

대부분의 게임 엔진은 직교좌표계(Cartesian Coordinate System)를 기본적으로 사용하지만, 극좌표계(Polar Coordinate System)도 특정한 경우에서 매우 유용하게 활용됩니다. 

게임에서는 보통 특정 좌표계를 그대로 사용하기보다, 필요할 때 해당 좌표계로 변환해서 사용합니다.

## 직교 좌표계

직교좌표계(Cartesian Coordinate System)는 서로 직각을 이루는 두 개 이상의 축을 기준으로 점의 위치를 표현하는 좌표계입니다.

축이 서로 수직이라서 직각삼각형의 피타고라스 정리를 쉽게 적용할 수 있습니다.  
벡터 연산과 행렬 변환에서 많이 사용됩니다.

직교좌표계에서는 회전 변환이 직관적이지 않습니다.

일반적인 2D 게임에서는 (x, y), 3D 게임에서는 (x, y, z) 형태로 표현됩니다.

직교좌표 → 극좌표 변환  
$r = \sqrt{x^2 + y^2}$  
$\theta = \tan^{-1} \left( \frac{y}{x} \right)$

게임에서 물체(캐릭터, 오브젝트, 카메라 등)의 위치를 정의할 때 기본적으로 직교좌표계를 사용합니다.

## 극 좌표계

극좌표계(Polar Coordinate System)는 물체의 위치를 원점으로부터의 거리(r)와 회전 각도(θ)로 표현하는 좌표계입니다.

각도(θ)를 기반으로 위치를 표현하므로, 회전 변환이 간단합니다.

(r, θ) 형태로 좌표를 정의합니다.

원점을 중심으로 회전하는 물체나 원형 패턴을 그리는 동작을 구현할 때 특히 유용합니다.

직관적이지 않으며, 좌표로의 변환이 필요합니다.

극좌표 → 직교좌표 변환  
$x = r \cos\theta$  
$y = r \sin\theta$

게임에서는 레이더 시스템, 총알 패턴, 카메라 회전, 로봇 팔 동작 등 다양한 곳에서 극좌표계를 활용할 수 있습니다.

예시 #1  
레이더 및 센서 시스템

+ 레이더 시스템은 방사형으로 탐지 범위를 설정하는 것이 일반적입니다.
+ 극좌표계에서는 원점을 중심으로 일정 거리 내에 있는 오브젝트를 쉽게 탐지할 수 있습니다.
+ 직교좌표계에서는 모든 오브젝트와의 거리를 계산해야 하지만, 극좌표계에서는 반지름 값만 비교하면 되므로 연산량이 줄어듭니다.

배틀로얄(Battle Royale) 장르의 게임에서는 안전 구역(세이프존)이 원형으로 설정되는 경우가 많으므로 해당 예시를 들어보겠습니다.  
이때 플레이어가 안전 구역 안에 있는지 확인하는 조건을 극좌표계로 쉽게 표현할 수 있습니다.

```cpp
#include <iostream>
#include <cmath>

using namespace std;

// 플레이어가 안전 구역 안에 있는지 확인하는 함수
bool isInSafeZone(double player_x, double player_y, double zone_center_x, double zone_center_y, double safe_radius) {
    // 플레이어와 안전 구역 중심 사이의 거리 계산
    double r = sqrt(pow(player_x - zone_center_x, 2) + pow(player_y - zone_center_y, 2));

    // 안전 구역 내부인지 확인
    return r <= safe_radius;
}

int main() {
    // 안전 구역 중심 (50, 50), 반지름 30
    double zone_x = 50, zone_y = 50, safe_radius = 30;
    
    // 플레이어 위치
    double player1_x = 60, player1_y = 60;
    double player2_x = 90, player2_y = 90;

    cout << "Player 1 is " << (isInSafeZone(player1_x, player1_y, zone_x, zone_y, safe_radius) ? "Safe" : "Not Safe") << endl;
    cout << "Player 2 is " << (isInSafeZone(player2_x, player2_y, zone_x, zone_y, safe_radius) ? "Safe" : "Not Safe") << endl;

    return 0;
}
```

안전 구역의 반지름을 $R_{\text{safe}}$ 라고 하고, 플레이어의 현재 위치를 극좌표계로 변환한 값이 $(r, \theta)$라고 하면, 플레이어가 원의 반지름 $R_{\text{safe}}$ 보다 작은 거리 안에 있다면 안전한 상태이고, 반지름보다 커지면 위험 지역(데미지를 받는 구역)으로 판정할 수 있습니다.

예시 #2  
슈팅 게임에서 적이 8개의 총알을 방사형으로 발사하는 기능

+ 각도(θ)를 조절하는 것만으로 쉽게 총알 배치를 구현할 수 있습니다.  
+ 직교좌표계에서 원형으로 총알을 배치하려면 복잡한 계산을 사용해야 하지만, 극좌표계에서는 각도를 일정하게 증가시키는 것만으로 간단하게 구현할 수 있습니다.

```cpp
#include <iostream>
#include <cmath>
#include <vector>

using namespace std;

// 총알 위치를 저장할 구조체
struct Bullet {
    double x, y;
};

// 총알의 위치를 극좌표계를 이용해 계산하는 함수
// 매개변수 n은 발사할 총알의 개수
vector<Bullet> generateBullets(int n, double radius) {
    vector<Bullet> bullets;
    
    // i는 총알의 인덱스
    for (int i = 0; i < n; i++) {
        double theta = (360.0 / n) * i; // 총알의 각도
        double radian = theta * M_PI / 180.0; // 각도를 라디안으로 변환
        
        double x = radius * cos(radian);
        double y = radius * sin(radian);
        
        bullets.push_back({x, y});
    }
    
    return bullets;
}

int main() {
    int bullet_count = 8;
    double radius = 10.0;
    
    vector<Bullet> bullets = generateBullets(bullet_count, radius);

    cout << "Bullet positions: " << endl;
    for (int i = 0; i < bullets.size(); i++) {
        cout << "Bullet " << i + 1 << ": (" << bullets[i].x << ", " << bullets[i].y << ")" << endl;
    }

    return 0;
}
```

각 총알은 일정한 간격(예: $45^\circ$)으로 배치되어야 합니다.  
이때 각 총알의 각도를  $\theta = \frac{360^\circ}{n} \times i$  로 계산할 수 있습니다.

$n = 발사할 총알 개수$(예: 8개)를 의미합니다.  
$i = 각 총알의 인덱스$(0부터 시작)를 의미합니다.

각 총알의 위치는 극좌표계를 사용하여 다음과 같이 계산됩니다.  
$x = r \cos(45^\circ \times i), \quad y = r \sin(45^\circ \times i)$
# [내일배움캠프 언리얼 5기] DAY 5 반응하는 레벨, 상호작용의 시작

## 1. 오늘 학습 키워드

>## 인터렉션 

>## 오버랩

>## 액터 이동 : Timeline

>## 히트

## 2. 오늘 학습 한 내용을 나만의 언어로 정리하기

### 1. 인터렉션 

## 1️⃣ 인터랙션이란 무엇인가?

- 게임에서 ‘인터랙션’은 단순한 누르기 그 이상을 말합니다.
- 플레이어가 **게임 세계에 손을 내밀고**, 그 세계가 **어떻게든 결과물을 돌려준다면**, 그것이 바로 인터랙션이라 할 수 있습니다.
    
- 유저가 흔히 만나는 인터랙션 예시들은 다음과 같습니다.
    - 직접적 상호작용: 문 열기, 버튼 누르기, 아이템 줍기
    - 환경적 상호작용: 함정 작동, 플랫폼 이동, 스위치 활성화
    - 물리적 상호작용: 오브젝트 밀기, 던지기, 파괴하기

## 2️⃣ 언리얼에서의 인터랙션은 “이벤트”

- **언리얼에서 인터랙션은 “이벤트”를 통해 발생합니다.**

    - 만약 문 앞에 플레이어가 있으면 문이 열리는 인터렉션을 구현하려면,
        1. Box Collision을 통해 캐릭터가 Collision 안에 들어오면 Overlap 감지합니다.
        2. 그러면 콜리전에 연결된 OnComponentBeginOverlap 이벤트가 발생합니다.
        3. EventGraph에 미리 설정한 Timeline 혹은 SetActorRotation 실행하게 됩니다.
        4. 그래서 결론적으로 문이 열리게 됩니다.
           
- 다음은 Blueprint의 EventGraph를 이루는 3가지 요소입니다.
    1. **Event 노드 (시작점):** BeginPlay, Tick, Overlap, Hit 등 이벤트가 어떻게 시작되는지 
    2. **조건 처리:** “어떤 액터지?”, “일정 수치 이상?” 등 여러 변수를 다양한 방법으로 참 거짓, 혹은 크기를 분류
    3. **결과 실행:** Timeline 재생, 문 열기, 파티클 (이펙트) 재생 등 결과물을 실행합니다.
  
## 3️⃣ 오버랩 vs. 충돌의 차이 이해하기

- **이 둘은 게임 안에서 세상과 플레이어가 만나는 방식들입니다. 겹치느냐 vs. 튕기느냐.** 이 차이가, 플레이어의 몰입감을 선사합니다.
    
- **🟢 오버랩 (Overlap)**
    - 액터들의 겹침을 감지하는 트리거 시스템이다
    - 물리 충돌 없이 통과 가능하다 (투명한 감지 영역 같은 느낌)
    - 실제 물리 반응은 없지만, 이벤트는 발생합니다.
    - 사용 예시: 문 앞 센서, 체크포인트, 아이템 습득 구역
- 🔴 **충돌 (Hit)**
    - 물리적 접촉을 감지합니다.
    - 튕김, 밀림, 파괴 등 물리 엔진 기반 반응이 가능합니다. 
    - 사용 예시: 벽에 부딪힘, 공 튀기기, 상자 밀기
- 실전 예시
    - 오버랩: 캐릭터가 특정 구역에 들어서자 → 자동문 열리는 구조로 만들 수 있습니다.
    - 충돌: 캐릭터가 상자를 부딪혀서 → 상자가 밀려나는 이벤트를 만들 수 있습니다.
    
- 오버랩과 충돌의 **차이, 한눈에 보기**
    
    
    | 항목 | 오버랩 (Overlap) | 충돌 (Hit) |
    | --- | --- | --- |
    | 통과가 가능한가? | ✔️ 예 | ❌ 아니오 |
    | 이벤트가 발생하는가? | ✔️ BeginOverlap 등 | ✔️ Hit 등 |
    | 물리 엔진 반응이 있는가? | ❌ 없음 | ✔️ 튕김, 힘 전달 |
    | 사용 목적 예시 | 감지, 트리거 | 물리 상호작용 |

### 2. 오버랩

## 오버랩 이벤트 (트리거) 감지 설정하기

1. 예시로 Capsule Collision에 캐릭터가 겹쳐질 경우 오버랩 이벤트가 발생하도록 설정을 해줘야합니다 .

2. 우선, `BP_SaveFlag` 블루프린트를 만들고, Collision 컴포넌트를 추가한 후, 우측에 Details 창에서 Collision 카테고리를 찾아봅니다.
    
<img width="1832" height="1297" alt="Image" src="https://github.com/user-attachments/assets/5c35ce57-88b8-47e5-9dc8-c51719120ac5" />
    
    - Generate Overlap Events 에 체크 표시를 합니다.
    - Collision Presets는 OverlapOnlyPawn으로 변경을 해줍니다.
    
4. 다음으로, Details 창에서 스크롤을 더 내려서 마지막에 Events 라는 카테고리가 보일 것입니다. 여기서 On Component Begin Overlap 옆에 + 버튼을 눌러줍시다.
    
<img width="1691" height="1169" alt="Image" src="https://github.com/user-attachments/assets/3cd65186-4566-4892-a702-d0fd84e70a22" />
    
5. 그러면 EventGraph 창으로 날아오면서 저절로 On Component Begin Overlap (Collision)이란 노드가 생성된 것을 볼 수 있습니다.

   이 노드는 어떤 캐릭터나 물체가 현재 Box Collision 겹치기 시작하는 순간 호출되는 이벤트입니다.

<img width="1979" height="1277" alt="Image" src="https://github.com/user-attachments/assets/f53808ea-63e6-44ef-99c0-ae8feaca9e38" />
    
6. 동일하게 다시 Components 창에서 Collision 컴포넌트를 클릭한 후, Details 창에서 Events 라는 카테고리가 On Component End Overlap 옆에 + 버튼도 눌러줍시다.
    
<img width="1691" height="1169" alt="Image" src="https://github.com/user-attachments/assets/3cd65186-4566-4892-a702-d0fd84e70a22" />
    
7. EventGraph 창에 On Component End Overlap (Collision) 노드도 생성되었습니다. 이 노드는 캐릭터나 물체가 Box Collision에서 겹쳐있다가 빠져나가는 순간 불리는 노드입니다.

<img width="1666" height="1379" alt="Image" src="https://github.com/user-attachments/assets/51934d36-31a0-4264-9b2f-e0d45d3eec19" />

### 3. 액터 이동 : Timeline

## 1️⃣ Timeline이란 무엇인가?

- **Timeline은 ‘시간’을 블루프린트 안에서 다룰 수 있게 해주는 기능**이다. 아래와 같은 기능을 **코드 없이 시각적으로** 구현할 수 있게 해주는 노드가 Timeline입니다.
    - 일정 시간 동안 조금씩 움직이기
    - 부드럽게 회전하기
    - 시간이 흐르면 변하기
- **Timeline이 하는 일**
    - Timeline은 **0부터 1까지**, 혹은 원하는 범위의 **값을 시간에 따라 변화시킵니다.**
      - **이동** (`SetActorLocation`)
      - **회전** (`SetActorRotation`)
      - **크기 변화** (`SetActorScale3D`)
      - **소리 볼륨**, **파티클 색상**, **UI 효과** 등 다양하게 제어할 수 있다.

## 2️⃣ Timeline + Lerp로 위치 이동 구현하기

1. `BP_MovingPlatform` 블루프린트를 만들고. 적절한 `StaticMesh`를 정한 후 Event Graph 창에서 타임라인을 쓰기 위해 블루프린트를 작업한다.

    우선, add timeline을 검색해서 타임라인 노드를 추간한다. 타임라인 노드가 어떤일을 하는지 쉽게 알아보기 위해 `Movement Timeline`이라고 변경하였다.
    
<img width="977" height="704" alt="Image" src="https://github.com/user-attachments/assets/93d7d852-fce1-4a97-aec1-7b3aa861191d" />
    
2. 이 `Movement Timeline` 노드를 더블 클릭하면 Timeline을 편집할 수 있는 창이 뜬다. ‘+ Track’ 이라는 버튼을 클릭해 ‘Add Float Track’을 추가한다. 
    
3. Track을 추가하면 아래와 같은 그래프가 나오게 되는데, 동시에 표시된 부분에서 이름을 바꿀수가 있다. 이 그래프의 이름을 `Alpha`로 저장한다. 

<img width="1949" height="1112" alt="Image" src="https://github.com/user-attachments/assets/e2730f54-6b04-41a9-ae95-95022a95e37c" />

4. 이번 코드에서는 그래프의 가로 X축은 시간 (Time)을 나타내고, 세로 Y축은 값 (Value)을 나타낸다. 다음으로 우클릭을 한 후 ‘Add key to CurveFloat_0’를 눌러서 키를 추가해준다. 키를 총 3개를 추가한다.
    
<img width="1879" height="1041" alt="Image" src="https://github.com/user-attachments/assets/75b5bb32-2981-4ced-ad1b-72125752dfc5" />
    
5. 그래프에 생긴 점을 이동을 하면, 현재 점 (키)의 Time 값과 Value 값을 볼 수 있는데 2개의 키를 아래와 같이 값을 설정한다.

    키를 클릭한 상태로 (클릭하면 하늘색) 상단에 있는 값 입력창에 값을 입력하고 엔터를 치면 된다. 

<img width="1651" height="752" alt="Image" src="https://github.com/user-attachments/assets/f07a3b5f-3d00-4c29-aa92-ce3b6c24ab4d" />
    
<img width="1658" height="838" alt="Image" src="https://github.com/user-attachments/assets/1126c79a-ef8f-4eef-bdf3-52d9dde9c02b" />

<img width="1868" height="904" alt="Image" src="https://github.com/user-attachments/assets/897d5a42-2f6f-4dc8-be18-17a36104dd20" />

    - 첫번째 키는 Time: 0, Value: 0으로 설정
    - 두번째 키는 Time: 2, Value: 1으로 설정
    - 세번째 키는 Time: 4, Value: 0으로 설정
    
6. 그래프를 좀 더 멀리서 보거나 확대해서 볼때는 마우스 휠을 돌려서 확인할 수 있다.

   좌측 상단에 Length 값도 4.00 으로 변경해서 그래프의 총 길이가 2초가 되도록 하여 한눈에 보기 편하게 설정해준다.

   그러면 다음과 같이 그래프가 완성된다. 4초에 걸쳐서 값이 0부터 1까지 올라갔다가 다시 0으로 변화하는 타임라인 그래프가 생성된다.

<img width="2006" height="1195" alt="Image" src="https://github.com/user-attachments/assets/ded1a93b-9a92-4feb-8701-77cb5474ed89" />
    
7. 마지막으로, 상단에 버튼들이 있는데, Use Last KeyFrame과 반복이 되는 Loop을 클릭해서 킨다다. (아래와 같이 하늘색이 되면 활성화) 
 
<img width="2047" height="1147" alt="Image" src="https://github.com/user-attachments/assets/a3e55eb8-50b0-4ad1-9d8b-cfe007fad067" />
    
8. 컴파일하고 저장을 한 후, 다시 EventGraph 창으로 돌아간다. 발판 액터의 시작 위치와 끝 위치를 저장한 Vector 변수를 아래와 같이 선언한다.
    
<img width="906" height="692" alt="Image" src="https://github.com/user-attachments/assets/f3f2d546-500a-448c-8aee-37f83cc90dbd" />

9. 다음으로 아래와 같이 블루프린트를 작성한다.
    
    <img width="1481" height="1023" alt="Image" src="https://github.com/user-attachments/assets/3f4ed47b-f4cc-4984-8bc7-bba038cddd32" />

    <img width="1510" height="1025" alt="Image" src="https://github.com/user-attachments/assets/067c9e46-5921-43e4-977c-4b6513f3ba38" />
    
    - Event BeginPlay
        - 게임이 시작되자마자 배치된 액터에서 호출되는 노드이다.
        - 발판의 위치값을 저장하고, 움직이기 위한 준비를 여기에서 진행한다.
    - Start Location을 설정한다. (GetActorLocation = Set Start Location)
        - 현재 액터의 위치를 가져와서 변수 `Start Location`에 저장한다.
        - “움직이기 시작할 원래 위치”를 기억하는 단계이다.
    - End Location을 설정한다 (Start Location + (0, 0, 300) = Set End Location)
        - `Start Location`에 `(0, 0, 300)`을 더한다. 즉 시작점에서 위로 300만큼 이동한 좌표이다.
        - 그 결과를 `End Location` 변수에 저장한다.
        - 즉, “도착 지점”을 계산해 저장하는 단계이다.
    - Timeline: Movement Timeline
        - 이 Timeline은 0~1까지의 값을 2초동안 바꾸는 곡선 (이 값이 Alpha로 출력)으로 설정되어있다.
        - Play from Start로 재생되고, Update 출력은 값이 계속 바뀌는 동안 매 프레임 실행된다.
    - Lerp (Vector)
        - Lerp는 A와 B 사이를 Alpha만큼 비율로 보간해주는 함수이다.
        - Alpha = 0이면 Start이고, Alpha = 1이면 En이다.
        - Timeline이 Alpha를 계속 변화시키면서 Lerp가 계속 중간값 계산해주는 역할은 한다.
    - Set Actor Location
        - Lerp 결과 위치를 사용해 실제로 액터를 이동시켜준다.
        - Timeline의 Update마다 호출되어 유저는 액터가 부드럽게 움직이는 것처럼 보이게 된다.

### 4. 히트  

## 1️⃣ 물리 반응하는 박스 액터 만들기

1. `BP_Bomb`이름을 가진 Blueprint Class를 생성한다.
    
<img width="1509" height="708" alt="Image" src="https://github.com/user-attachments/assets/3e370968-47b6-490d-93a4-a5f06b35d086" />
    
2. `BP_Bomb` 블루프린트로 들어가서 Components 창에서 ‘+ Add’ 버튼을 클릭하고 Static Mesh 컴포넌트를 검색해서 추가한다. 이름은 `StaticMesh`로 냅둔다.
    
<img width="1211" height="852" alt="Image" src="https://github.com/user-attachments/assets/d727f4e1-5ddd-4167-a630-6054cf250cb9" />
    
3. `StaticMesh` 컴포넌트를 선택하고 우측의 Details 창에서 Static Mesh 카테고리의 Static Mesh 속성을 `Cylinder`로 지정한다.
    
4. 물리 설정을 활성화하기 위해서, Details 창에서 Physics 카테고리를 찾아 Simulate Physics 속성을 체크한다. Mass에 체크하고 1000.0으로 설정하고, Linear Damping과 Angular Damping도 0.5로 값을 각각 설정했다.

<img width="1542" height="928" alt="Image" src="https://github.com/user-attachments/assets/b06d02e7-ca91-44a9-bee9-cc1aade00568" />
    
5. Details 창에서 스크롤을 더 내려서 Collision 카테고리에서 Collision Presets가 PhysicsActor로 설정되어 있는지 확인을 한다.

<img width="1497" height="1033" alt="Image" src="https://github.com/user-attachments/assets/30dd6d0e-462e-4ac1-9e75-cb90131753f3" />
    
6. `BP_Bomb` 블루프린트를 컴파일하고 저장을 한다. 이후 에디터 뷰포트에서 `BP_Bomb`를 드래그하여 레벨에 배치하고, 물리적인 반응을 하는지 테스트를 해본다.

<img width="1272" height="954" alt="Image" src="https://github.com/user-attachments/assets/ecc9673f-5ed5-4e4b-9538-9a8f16812c5e" />

<img width="1575" height="1068" alt="Image" src="https://github.com/user-attachments/assets/4da6f411-8a77-483c-a02d-4e1581257dfb" />


## 2️⃣ 충돌 이벤트 감지에 따른 반응 추가하기

1. `BP_Bomb` 블루프린트로 다시 돌아가 Components 창에서 StaticMesh 컴포넌트를 선택한다.

    Details 창에서 맨 아래로 내려서 Events 카테고리를에서 On Component Hit를 찾고 옆에 + 버튼을 눌러 Event Graph에 이벤트 노드를 추가한다.

<img width="2468" height="902" alt="Image" src="https://github.com/user-attachments/assets/72c51e53-1c20-4566-a28e-dd96917e2b76" />
    
2. Event Graph 창으로 넘어가면서 아래와 같은 노드가 생성된 것을 볼 수 있다.

<img width="2559" height="703" alt="Image" src="https://github.com/user-attachments/assets/4fcf1d7f-b4b1-43b1-902e-af9b60d63773" />
    
3. 실린더에 닿으면 플레이어가 튀어오르게 하기 위해 다음의 노드를 사용하여 블루프린트 로직을 작성한다.

    <img width="1864" height="998" alt="Image" src="https://github.com/user-attachments/assets/b3893b0b-bdf9-44e2-a268-577d007ce4af" />
    
    - OnComponentHit (StaticMesh)
        - Static Mesh에 물리적 충돌이 감지될 때 실행되는 이벤트이다.
        - 메시에 표면에 다른 뭔가가 부딪히면 호출이 된다.
        - 충돌 대상에 대한 정보(Other Actor 등)가 이벤트로 흘러 들어오게 된다.
    - Cast To BP_Character
        - 충돌한 대상이 우리 게임에서 쓰는 플레이어 캐릭터인지 확인한다.
        - 다른 오브젝트랑 충돌하면 무시한다.
        - 충돌한 Actor를 BP Character로 Cast 성공한다면, 충돌 주체가 캐릭터임을 확인 할 수 있다.
    - Launch Character
        - Launch = 플레이어를 어느 방향으로 발사 하는 힘이다.
        - `Z = 10000`이면 위로 팍 튀어오르게 된다.
        - `Target = As BP Character`로 설정하여 Hit가 발생했을때 닿은 상대가 플레이어면 작용하게 합니다.
    - 만약 플레이어가 아니라면 Add Impulse 사용한다
        - Impulse = 아무 액터나 순간적으로 튀게 하는 힘을 줄 수 있다.
        - `Target = StaticMesh`로 해서 블루프린트가 소유한 메쉬 자체에 힘을 주게 한다.
          
6. `BP_PhysicsBox` 블루프린트를 컴파일하고 저장한 후, 뷰포트에서 게임 플레이를 실행하고 테스트해봅시다. 박스와 캐릭터가 부딪히면 멀리 날아가버리는 효과를 볼 수 있습니다.


<img width="1663" height="888" alt="Image" src="https://github.com/user-attachments/assets/88066e8b-0c92-4f11-a960-ac6b678e8bff" />

<img width="1903" height="909" alt="Image" src="https://github.com/user-attachments/assets/1a6fd74b-52d8-4c45-9ba8-597e02602e20" />

#내일배움캠프 #사전캠프 #TIL 

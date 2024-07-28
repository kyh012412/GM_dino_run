https://www.youtube.com/playlist?list=PLO-mt5Iu5TeaMl--ItJ2so8BitO_BfTIz

### 공룡 런게임 - ⚡ 볼트 시작하기 [V01]

#### 볼트 다운로드

1. 볼트는 다운로드 받을 필요가없으며
2. visual scripting이라는 것으로 이미 다운로드가 된듯
3. camera 내에서 (flow graph)script machine을 추가해준다.
   1. Macro 타입은 에셋으로 생성필요(참고)
   2. 임베드(Embed) 타입은 오브젝트 내에 즉시 편집 가능
   3. source 내에 Embed로 바꾸고 Edit graph를 눌러준다.
   4. 거대한 그래프 (start와 update가 보인다)
   5. 빈공간에서 우클릭하면 만들수있는것들이나오고
   6. log를 검색해서 debug.log상자를 추가해준다.
   7. 화살표 (컨트롤이라고 칭함) : 볼트의 핵심 요소, 로직의 흐름
   8. start에서 log로 선을 이어줌
   9. ![[Pasted image 20240726210615.png]]
   10. 볼트의 특징 : 유닛이 요구하는 데이터가 없으면 *노랑으로 경고*가 발생함
   11. 다시 add unit (빈공간 우클릭) > add node > string literal
   12. 리터럴 : 타입의 순수한 형태, 코드의 변수같은 역할
   13. ![[Pasted image 20240726210914.png]]
   14. flow graph(script graph)탭은 콘솔 옆탭으로 옴겨준다.
   15. ![[Pasted image 20240726211226.png]]
   16. 디버깅을 하지 않아도 값의 이동이 보임
       1. 디버깅이 쉽다.
   17.

### 공룡 런게임 - 플레이어 점프[V02+Asset]

[공룡 런게임 에셋 링크](https://assetstore.unity.com/packages/2d/characters/bolt-2d-dinorun-assets-pack-188721)

#### 오브젝트만들기

1. run 0 을 하이라키에 넣어주고
   1. 이름과 위치 수정
2. Animation 관련
   1. Assets 이하에 Animation 폴더를 만들어주고
   2. Animation 폴더에 Animation Controller를 만들어준다.
   3. 그리고 빈 Animation 3개를 만들어준다. (Run Jump Die)
   4. Animator 탭에서 3개의애니메이션을 넣어준다.
   5. Run이 Default state
   6. Run 과 Jump는 양방향
   7. any state > die (트리거 추가예정)
   8. 파라미터
      1. isJump(bool)
      2. doDie(trigger)
   9. transition to die
      1. transition 0
      2. has exit time 비활성화
      3. condition doDie
   10. run < > jump
       1. isjump에 따른 제어
       2. 비활성화
       3. condition
   11. Ctrl+ 6으로 animation 탭을열고
   12. 드래그드랍으로 sprite 추가
3. rigidbody2d 와 circle collider 2d 추가
   1. rigidbody2d 내에 freeze rotation z를 체크
   2. circle collider 2d 반지름 0.4
4. audio source 추가
   1. 볼륨 0.4
   2. play on awake
5. ground 추가
   1. y 값 -3
   2. rigid body 2d
      1. body type static
   3. box collider 2d

#### 매크로와 임베드

1. 플레이어 오브젝트에 플로우 머신 추가
2. 플레이어 오브젝트에 플로우머신(scprit machine) 추가
3. graph에 new를 눌러서 Assets/Macros 내에 저장 (Player.asset)

##### 매크로와 임베드의 차이점

1. 임베트
   1. 임베드로 만든 스크립트 머신은
   2. 하이라키에 잇는 오브젝트를 끌어와서 연결할 수 있다.
   3. 임베드가 포함된 오브젝트를 삭제하면 복구가 어렵다.
2. 매크로
   1. 매크로는 장면 오브젝트에 접근이 불가능 (=프리펩)
   2. 매크로는 프리펩과같아서 오브젝트가 지워져도 복구가 가능하다.

#### 점프 구현

1. Player (script machine)에서 우클릭 > event > input > On Button Input
   1. Name Jump
   2. Action Down
2. 녹색상자 >
   1. 컨트롤 유닛 : 그래프 흐름이 시작되는 유닛
   2. 컨트롤 유닛은 대부분 Events 내에 있음 (정독이 요구됨)
3. 카메라의
   1. background 색은 회색
   2. 사이즈 3
4. 테스트
   1. 컨트롤 유닛의 색이 일시적으로 파란색이 됨을 눈으로확인
5. 새로운 유닛(노드) 를 검색해서 추가
   1. 검색어 setbool (대소문자 구분x)
6. 노드를 추가했을때 노란불이 들어오지 않는다는 것은
   1. 플레이어 오브젝트내에 animator를 인식했다는 뜻
7. 볼트의 특징 컴포넌트를 자동으로 감지해서 셋팅
8. 새로운 노드 추가
   1. audio play 검색하여 audio source play를 추가
9. 새로운 노드 추가 오디오 클립
   1. audio source : set clip 을 추가
   2. 음악을 asset으로 부터 가져와서 연결
10. set bool > set clip > play 순으로 연결
11. add force를 검색하여
    1. rigidbody 2d / add force mode
    2. ![[Pasted image 20240727121546.png]]
12. _매크로는 플레이 도중 수정하고 종료해도 그대로 저장_
13. 새로운 노드 검색 set gravity
    1. Physics 2d, Set gravity를찾아서
    2. 0,-20을 넣어준뒤 start로 부터 연결
    3. ![[Pasted image 20240727122112.png]]
    4. 기본값은 (0,-9.81)
14. 새로운 노드
    1. Event > Event > Physics2D 접근
    2. On Collision Enter 2D
    3. ![[Pasted image 20240727122648.png]]
15. 이 세개를 선택후 Ctrl + D를 눌러서 복사해준다. 5. ![[Pasted image 20240727122637.png]] 6. 다음 처럼 연결 7. ![[Pasted image 20240727122747.png]]

#### 다듬기

1. 카메라 컬러 414346
2. Player collider offset y값을 > 0.4로 변경
3. Assets/Animation/Run.asset의 loop time을 체크

### 공룡 런게임 - 🏜️무한 배경을 위한 스크롤링 & 패럴랙스 [V03]

#### 배경 구성

1. 기존에 만들어둔 땅
   1. 스프라이트는 비활성화
   2. 이름 Floor
2. 빈 게임 오브젝트를 만들어준다.(Sky Group)
   1. 위치 초기화
   2. 복제하여 총 4개가 되도록 생성(Sky, Cloud, Mountain, Ground)
3. 각자에 해당하는 오브젝트를 채워넣어준다.
   1. Mountain > Back A를
   2. Ground는 y값 -3
   3. sky Renderer 밑의 order in layer 값을 -3
   4. cloud -2 , y값 2
   5. back -1

#### 무한 배경

1. ground 오브젝트 내에서 script machine(macro) 생성
2. 노드 추가
   1. get pos 검색으로
   2. transform : get position을 가져옴
3. 노드 추가
   1. get x 검색으로
   2. Vector3 : get x를 가져옴
   3. ![[Pasted image 20240727140020.png]]
4. 우클릭하여 Logic 이라는 카테고리이용
   1. ![[Pasted image 20240727140104.png]]
5. log > less
   1. ![[Pasted image 20240727140244.png]]
6. control > branch (if)
   1. ![[Pasted image 20240727140501.png]]
7. get local pos
   1. ![[Pasted image 20240727140542.png]]
8. Vector3
   1. literal
   2. ![[Pasted image 20240727140633.png]]
9. Math 카테고리 Vector3 > Add
   1. ![[Pasted image 20240727140720.png]]
10. Set local pos
    1. ![[Pasted image 20240727141026.png]]
11. 전체 흐름 1
    1. ![[Pasted image 20240727141119.png]]
12. 전체 흐름 2
    1. ![[Pasted image 20240727141131.png]]
13. 테스트
    1. 자식 객체들에 적용을 하지만
    2. 움직이는 것은 Group이 움직임
    3. 여기까지 정상이면 다른 자식 객체들에도 동일한 Reposition을 적용
14. ground를 7번복사하여 8개 까지 만들어줌(group x ground o)
    1. sky도 동일
15. 산의 경우는 (back) 2개를 더 추가해주고 Back B로 변경 후 적당히 배치
    1. 구름도 동일
16. 모든 그룹을 x값을 -9해준다.
    1. 스카이 그룹은 y값 -1해준다.

#### 스크롤링

1. 배경 그룹 오브젝트에 스크립트 머신과 메크로 추가 (Scrolling)
2. Transform.translate라는 노드를 추가해준다.
3. 델타 타임도 고려해준다.
4. Math Scalar > Multiply 유닛 추가
5. ![[Pasted image 20240727164233.png]]
6. 하늘을 제외한 다른 그룹쪽에도 적용해준다.
7. 테스트
   1. 문제
      1. 원근감이 고려되지 않아서 산과 구름이 등속도로 이동중

#### 패럴랙스

1. 볼트(Visual scripting)을 사용하면 자동으로 Scene Variables 가 포함됨
2. script machine을 쓰면 동시에 Varialbes 라는 컴포넌트도 사용됨 (기능을 지원)
3. ~~window > variables 이 있음~~
4. ![[Pasted image 20240727165739.png]]
5. Script Graph 내에서 \<x> 버튼을 눌러서 blackboard를열고 여기에서 작업을하면된다.
6. Scene 단위에서 쓸 변수 Speed 를 만들어주고 float -1
7. Script Graph 내에서 Variables > Scene > get Speed
   1. ![[Pasted image 20240727170055.png]]
8. 3개의 그룹이 같은 Scrolling을 쓰고있으므로
9. 클라우드그룹에서
10. Macro(graph) 옆에 convert를 눌러서 변환을 해준다.
11. 연결을 끈고 중간다리로
    1. math > scalar > multiply를 연결해준다.
    2. ![[Pasted image 20240727170346.png]]
12. 마운틴 그룹은 동일하지만 스피드를 0.1을 준다.
13. _패럴랙스 : 거리에 따라 속도를 달리하여 원근감을 주는 기술_

### 공룡 런게임 - 🚧장애물 만들기 [V04]

#### 중복 그래프

1. Scrolling.asset을 다른곳에서도 사용할수 있게 만들기
2. Update와 speed 를 지워준다.
3. add node
   1. nesting > input
4. inpuy node를 클릭한상태에서
   1. ![[Pasted image 20240727171808.png]]
5. 느낌표를 눌러서 graph inspector를 열어준다.
   1. Control input = (Trigger Inputs)
   2. Value Inputs = Data Inputs
6. Input 유닛 : 유닛 진입에 필요한 컨트롤, 데이터를 정하는 유닛
7. ![[Pasted image 20240727173156.png]]
8. ![[Pasted image 20240727173206.png]]
9. 슈퍼 유닛 : 다른 그래프 내부에 사용하기 위한 매크로
10. 이제 Scrolling은 슈퍼 유닛이 되었음
11. 그라운드 그룹으로 이동
12. ![[Pasted image 20240727173355.png]]
13. 이렇게 남기고 다 지운뒤
14. Scrolling 매크로를 넣어준다.
15. ![[Pasted image 20240727173616.png]]
16. 연결
17. 이하 다른 그룹들도 동일하게 만들어준다.
18. 슈퍼 유닛으로 메모리 절약과 체계적인 유지보수 가능

#### 슈퍼 유닛 활용

1. resposition.asset
2. ![[Pasted image 20240727173948.png]]
3. 도 다른곳에서 추후에 활용하기를 원함 (선인장 등)
4. 에셋 창에서 새롭게 Script machine을 만들어준다.
5. reposition에서 update를 제외한 모든것을 ctrl + x로 잘라내서
6. 새 매크로에 붙여넣기
7. 동일하게 input 유닛만들어주기
   1. 이름 in
   2. hide label 체크
8. 추가적으로 nesting > output을 추가해준다.
9. _output 유닛 추가하여 컨트롤이 끝나는 지점에 연결_
10. ![[Pasted image 20240727174510.png]]
11. 다시 reposition.asset내부에서 다시 연결해준다.
12. ![[Pasted image 20240727174618.png]]
13. 로직이 잘렸던 기존 매크로에 슈퍼 유닛을 붙여서 복원
14. 테스트
    1. ok

#### 장애물 만들기

1. 장애물도 Ground Group 내에 추가예정
2. 3종류 의 스프라이트를 각각 객체로 만들어준다.
   1. A,B,C의 위치를 22 30 38로 해준다.
   2. Cactus A
      1. x위치 1
      2. Box collider 2d
         1. size
            1. x 0.6
            2. y 1
   3. Cactus B
      1. x 위치 9
      2. BoxCollider 2d
         1. size
         2. size
            1. x 1.1
            2. y 0.7
   4. Cactus C
      1. x 위치 17
      2. _복잡한 문양의 collider를따는방법_
      3. Polygon Collider 2d
      4. 폴리곤 콜라이더는 연산량이 커서 많이 사용하지 않도록 주의
   5. Cactus 에 script machine을 추가해준다.
   6. graph(macro)에서 Cactus.asset을 만들어 준다.
   7. move side를 추가해주고
   8. 새 노드 추가
      1. ran 검새긍로
      2. Random.Range 유닛을 추가
         1. 매개변수 자료형이 int 인것으로 선택
   9. equal 노드 추가
      1. graph inspector에서 numeric을 체크
   10. sprite renderer set enable 검색 후 노드추가
   11.
   12. box collider 2d와 polygon collider 2d를 같이 쓸수있게
       1. Collider 2d set enabled를 노드를 추가해준다.
   13. 연결
       1. ![[Pasted image 20240728104527.png]]

#### 충돌 이벤트

1. Player의 scriptr machine 내에서
   1. Events > Physics 2d > on trigger enter 2d 로 노드 추가
   2. 충돌 로직은 추후에
   3. 현재는 검증용으로 debug log까지만
2. debug log 노드추가
3. string literal 추가
4. 속도 가 느리므로 Scene 변수에 Speed를 -2~-5 (-5)로 변경
5. 테스트
   1. 정상작동
   2. 추가 버그
   3. 연속 점프가 가능한 상태

##### 연속 점프 제어하기

1. animator get bool과
2. if문을 가져와서 연결
3. ![[Pasted image 20240728111053.png]]

### 공룡 런게임 - 점수 시스템 구현 [V05]

#### 점수 계산

1. 게임 매니저 오브젝트를 만들어준다.
2. script machine을 추가해준다. (GameManager.asset)
3. deltaTime을 5배해주고
4. GameTime이라는 변수를 생성해준다.(float)
5. _컴포넌트에 있는 variables에서 변수를 만들면 object급의 변수가 만들어진다._
6. ![[Pasted image 20240728112447.png]]
7. 점수 변수를 만들것인데
8. 다른 오브젝트에서도 사용가능해야하기 때문에 scene급에서 변수를 만들어준다.
9. Score (Integer)
10. ![[Pasted image 20240728114550.png]]

#### UI 만들기

1. 캔버스 > text 추가
2. Canvas > Render Mode
   1. overlay에서 Camera로 변경
   2. 아래에 Render Camera가 생기는데
      1. main camera를 드래그해서 넣어준다.
3. Text
   1. 앵커 우상단
   2. 라벨 0000
   3. font 70
   4. bold
   5. overflow, overflow
   6. 중앙정렬, 중앙정렬
   7. 색상 하얀색
4. Text -animator
   1. 에 쓸 ScoreText 라는 animator 하나와
   2. animation(Check)를 추가해준다.
   3. doCheck라는 파라미터를 추가
   4. Empty State추가
   5. Empty > Check 의 컨디션을 넣어주고
   6. Exit time 비활성화
   7. transition 0
   8. 양방향 duration 0
   9. 돌아갈때는
      1. Has Exit time 체크
      2. ,transition 0.25
5. Check animation 내에서
   1. Scale property를 추가해주고
   2. 10프레임부터 50프레임까지 scale을 1.3으로 잡아준다.
   3. 0과 60은 scale 1

#### 점수표시

1. Text에 State Machine을 추가해준다.
2. Score라는 매크로(graph)를 만들어준다.
3. (State Machine)은 아이콘이 조금 다름
4. 스테이트 유닛 : 머신의 상태에 따라 실행되는 유닛
5. Flow machine(Script machine)을 감싸고 있는 하나의 큰 카테고리
6. Start 라고 써잇는 부분을 (graph inspector) 에서 이름을 편집하여
7. Scoring이라고 이름을 바꿔준다.
8. State 유닛 더블릭클릭으로 들어가기
9. set Text 검새해서 유닛 추가
   1. string을 매개변수로받기에
   2. format 필요
10. string format 유닛 추가
11. ![[Pasted image 20240728133326.png]]

#### 점수 표시

1. Text 객체내에
2. Object 단위로 NextCheck라는 int형 변수 만들어주기
3. 100점 씩 체크 포인트를 만들것이기에
4. 초기값을 100을 넣어준다.
5. NextCheck 의 값을 가져오는 유닛 추가
6. Equal 유닛 추가
   1. Equal은 기본값이 객체비교이기에
   2. graph inspector에 가서 Numeric을 체크해준다.
7. 만약에 score와 nextCheck가 같다면 nextcheck +=100을 해주는 로직을 만든다.
8. ![[Pasted image 20240728134723.png]]
9. 다시 Score State machine까지 가서
10. Scoring이 보이는 곳까지가서
11. 우클릭하여 create script state 눌러준다.
    1. Title Checking
    2. On Enter State 제외 삭제
       1. 코딩할때는 없었던
       2. 스테이트 유닛에 진입할 때 실행되는 유닛
    3. ![[Pasted image 20240728142110.png]]
12. Scoring 에서 make transition을 하여 Checking으로 연결
    1. ![[Pasted image 20240728142304.png]]
13. 노란색 (No Event)를 클릭하여 들어가보면
14. Trigger Transition : 상태 변경을 위해 컨트롤을 받는 유닛
15. Events > UnityEvent 유닛을 추가하여
    1. Check라고 이름을 지어준다.
    2. ![[Pasted image 20240728142518.png]]
16. 다시 Scoring 내로 들어가서
17. trigger event를 검색하면
    1. script 관련이잇고
    2. state 관련이 잇다.
    3. 현재는 state 관련을 써야한다.
    4. ![[Pasted image 20240728142710.png]]
18. 문제
    1. Scoring에서 Checking으로 편도 방향이다.
19. animator 내에 check animation으로가서 40프레임쯤에 add event로 이벤트를 추가해준다.
    1. 이제 inspector 창에 가서
    2. state machine > methods > TriggerAnimationEvent라는 것이 있음
    3. String 에 "Score" 를 적어준뒤
    4. State Machine으로 돌아와서
20. 돌아가는 transition 만들어준뒤 6. 돌아가는 event 내에서 7. Events > Animation > Named Anim Event 추가 8. ![[Pasted image 20240728143831.png]] 9.

### 공룡 런게임 - 게임 완성하기 [V06]

#### 게임오버 이벤트

1. Player Script graph 내에서
2. on Tirigger Enter 2d를 수정
3. ![[Pasted image 20240728190047.png]]
4. (위에 바닥 착지시 발생하는 로직을 복사해서 일부만 수정하는쪽이 편함)
5. unit을 오른클릭후 replace로 교체할수 있다.
6. rigidbody 2d > set simulated 유닛 추가
7. ![[Pasted image 20240728190546.png]]
8. 이렇게 연결시 더이상 rigidbody가 동작하지 않음
9. Scene 단계 bool 변수 추가 (Live)
   1. 기본값 true
10. ![[Pasted image 20240728190836.png]]
11. Jump 로직도 live가 true일때만 가능하게 수정
12. ![[Pasted image 20240728190942.png]]
13. GameManager에서도
14. live가 true일때 로직 수행
    1. ![[Pasted image 20240728191144.png]]
15. macros 폴더내 Scrolling에도 동일
16. ![[Pasted image 20240728191341.png]]
17. 테스트
    1. 정상

#### 게임 재시작

1. 캔버스내에 GameOver 빈객체 추가
2. GameOver 내에 Image추가
   1. 이미지소스 UI Over
   2. set native size
   3. 이미지가 반만보이는문제
   4. Canvas 객체의 Canvas 컴포넌트내에 Order in layer 가 있다. > 10
   5. posy 55
3. GameOver 내에서 Button 추가
   1. 버튼내 텍스트 제거
   2. 이미지 소스 UI Btn On
   3. set native size
   4. pos y -70
   5. button 컴포넌트 내에서
      1. Transition 값이 Color Tint 인데
      2. > Sprite Swap으로 변경
      3. Pressed 와 Selected 는 UI Btn Down 으로 변경
   6. 이하에 On Click에 추가를 해주고
      1. 객체에는 GameManager를 드래그해서 등록
      2. 함수 설정에서 Script Machine > TriggerUnityEvent
      3. 함수이름을 Reset으로지어준다.
4. Game Manager의 Script Machine으로 와서
5. 유닛추가
   1. Events > UnityEvent 를 추가해주고
   2. 내부에 Reset(string)을 넣어준다.
6. GameManger(객체) 내에 오디오 소스를 추가해준다.
   1. clip은 Button이라는 클립을 넣어준다.
   2. play on awake 체크 해제
   3. 볼륨 0.4
7. SceneManager loadScene 유닛 추가
   1. 파라미터네 SceneName(string) 인 매개변수 한개만 있는것으로
8. 만약에 이렇게 연결한다면
   1. ![[Pasted image 20240728200048.png]]
   2. 소리는 나지않음 (거의 동시에 발생하기에)
   3. 딜레이가 필요
9. Timer 유닛 추가
   1. duration에 딜레이 원하는만큼의 숫자(seconds)를 적어준다.
10. LoadScene하기전에
    1. file>settings 에서
    2. add opens scene를 통해 scene를 추가해줘야한다.
11. GameOver 오브젝트는 비활성화
12. GameManager의 script machine에서
    1. ctrl + 드래그로 그룹을만들어줄수있다.
    2. ![[Pasted image 20240728200812.png]]
    3. 이름을 Scoring으로 변경
    4. Object 단위의 변수를 추가해준다.
       1. GameOverUI
       2. type 은 GameObject
       3. GameOver(객체)를 드래그해서 넣어준다.
    5. GameObject set active 유닛을추가해주고
    6. 연결
13. GameManager내에서 Live가 false일때 UI를 켜기
    1. ![[Pasted image 20240728201343.png]]
    2. 이렇게하면 매초 60번의 콜을 받게 되니 비효율적
14. Control > Once 유닛을추가해준다.
    1. ![[Pasted image 20240728201613.png]]
15. 테스트
    1. 정상

#### 하이 스코어

1. Score Text를 ctrl D를 써서 복사(HiScore Text)
   1. 위치를 좌측으로 드래그
   2. 색상 0f6ed4
   3. script machine을 삭제
   4. 새로운 script machine
   5. 임베드로 제작
   6. high score 변수를 saved에서 만듬 (int)
   7. 여기서 만든 데이터는 저장이됨
   8. ![[Pasted image 20240728204031.png]]
2. GameManager에서 최고스코어 갱신로직
   1. Scene내의 Score와
   2. Saved 내의 HiScore를 가져오고
   3. 비교를위해
   4. Logic > greater를 선택
   5. GameOver UI 이후에 이어서
   6. ![[Pasted image 20240728204533.png]]
3. Saved 변수 만드는 것에 대한 지식
   1. Initial 이 있고 Saved 가 있는데
   2. Initial는 초기값 Saved는 저장된 값이다.
4. 테스트
   1. 정상

#### 레벨 디자인

1. 레벨 디자인은 복잡할 수 있기에 그래프가 아닌 스크립트에서 조작
2. GameManager.cs

   ```cs
   public class GameManager : MonoBehaviour
   {
       public float level(int score){
           int defaultSpeed = -5;
           int increSpeed = (score * -1) / 100;
           if(increSpeed < -5){
               increSpeed = -5;
           }

           return defaultSpeed+increSpeed;
       }
   }
   ```

3. GameManager객체에 컴포넌트를 추가해주고
4. ~~Tool > Bolt > Update Unit Option 실행~~
5. Edit > Project Setting > Visual Scripting >
   1. Regenerate Nodes (가로로 긴버튼) 클릭
6. 새로운 유닛 추가
   1. GameManager > Game manager > level
   2. ![[Pasted image 20240728211514.png]]
   3. GameManager 의 Scoring 뒤에 붙여준다.
7. 테스트

### https://www.youtube.com/watch?v=W0Tf8DuMwbQ&list=PLO-mt5Iu5TeaMl--ItJ2so8BitO_BfTIz&index=7&pp=iAQB 차례

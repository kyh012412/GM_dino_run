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

###

# SMUtime

  SMUtime은 상명대학교 학생들을 위한 편의기능을 제공하는 커뮤니티이다.
  
# SMUtime의 주요 기능

    - 회원가입/로그인
    - 게시판
    - 시간표
    - 친구추가 및 목록/채팅
    - 셔틀버스시간표
    - 버스시간표
    - 택시 모집

# SMUtime 구성도
![화면 구성](https://user-images.githubusercontent.com/80017979/121213328-17478480-c8b9-11eb-8e22-ff5b9b712788.jpg)

# 기능 구현

<details>
    
   <summary> - 회원가입/로그인(마혜준)</summary>
  
  회원가입과 로그인에서는 Firebase Authentication과 Firebase Realtime Database를 이용해 구현하였다.
  ### 1.회원가입

회원가입의 주요 기능은 다음과 같다.

    1) 회원가입 형식 설정(입력창 빈칸이 있는지, 이메일/비밀번호 형식을 지켰는지, 상명대 웹메일 도메인 주소를 사용했는지 등)
    2) 입력한 이메일과 비밀번호 Firebase Authentication에 저장
    3) 입력한 정보(이메일, 비밀번호, 닉네임) Firebase Realtime Database에 저장
    4) 가입한 이메일이 허위인지 아닌지를 구별할 수 있도록 이메일 인증 메일 전송

사용 파일 :

    - activity_register.xml : 회원가입 레이아웃 작성
    - RegisterActivity.java : 회원가입을 실행하는 코드 작성
    - User.java : 회원정보(이메일, 비밀번호, 닉네임)이 들어갈 DB

  > ### activity_register.xml
  
  #### 레이아웃 화면
   ![image](https://user-images.githubusercontent.com/70474860/121330595-1d893f80-c951-11eb-9d06-625384247dd6.png)
   
  #### 코드
   ![image](https://user-images.githubusercontent.com/70474860/121328667-7ce65000-c94f-11eb-95c3-d47aa4a1266b.png)


  > ### RegisterActivity.java
  
  #### import 및 변수 선언
  ![image](https://user-images.githubusercontent.com/70474860/121331553-f1ba8980-c951-11eb-8abb-c152a2e7a477.png)

  #### onCreate() 메소드
![image](https://user-images.githubusercontent.com/70474860/121331691-16aefc80-c952-11eb-87ab-2504b4c126fd.png)
activity_register.xml에 있는 객체들을 변수와 link.
OnClickListener 호출.
  btnEmail 버튼 클릭 시 emailCheck() 메소드 호출
  btnSubmit 버튼 클릭 시 조건(if(IsValidPwd()==true && IsPwdChecked()==true && IsEmptyName()==false))에 따라 createNewUser() 메소드 호출
  
  #### emailcheck() 메소드 -> btnEmail 클릭 시 호출
![image](https://user-images.githubusercontent.com/70474860/121333020-5de9bd00-c953-11eb-9e2b-270e5bd2b2c1.png)
editRegisterEmail의 입력에 따라 결과 설정

    잘못 입력했을 경우: 해당 editText에 focus되며, "가입" 버튼을 비활성화. 비활성화된 것을 시각적으로 표현하기 위해 backgroundColor를 회색으로 설정.
    정상적으로 입력했을 경우: btnSubmit 버튼이 활성화됨. 활성화된 것을 시각적으로 표현하기 위해 backgroundColor를 파란색으로 설정.

  #### IsValidPwd() 메소드
![image](https://user-images.githubusercontent.com/70474860/121334197-6c84a400-c954-11eb-9201-26dcc6f63b5d.png)
보안강화를 위해 비밀번호 정규표현식을 설정하였다. 
editRegisterPwd에 입력한 값이 대소문자, 숫자, 특수문자를 모두 포함하고 8자 이상이어야 isValidPwd() 메소드가 true를 반환.

  #### IsValidPwdCheck() 메소드
![image](https://user-images.githubusercontent.com/70474860/121335013-2c71f100-c955-11eb-8e7d-edece8a7a2b8.png)
editRegisterPwd 입력값과 editRegisterPwdCheck 입력값이 값아야 IsValidPwdCheck() 메소드가 true를 반환.

  #### IsEmptyName() 메소드
![image](https://user-images.githubusercontent.com/70474860/121335391-807cd580-c955-11eb-841a-9138b4a1a629.png)
editRegisterName에 입력값이 있어야 IsEmptyName() 메소드가 true를 반환.

  #### createNewUser() 메소드 -> btnSubmit 클릭 시 세 메소드 IsValidPwd(), IsValidPwdCheck(), IsEmptyName()의 반환값이 모두 true일 때 호출
![image](https://user-images.githubusercontent.com/70474860/121336120-2d575280-c956-11eb-9c69-6547639eba31.png)
mAuth.createUserWithEmailAndPassword(email, password).addOnCompleteListener에서 입력한 email과 password를 Firebase Authentication에 계정 생성. 만약, 이미 있는 계정이면 이미 등록된 계정이라고 뜸.
계정 생성에 성공하면 User.java를 호출하여 해당 입력값을 Realtime Database에 저장.
저장 후, 해당하는 이메일 주소로 인증 메일 발송.

  > ### User.java
![image](https://user-images.githubusercontent.com/70474860/121337480-8a9fd380-c957-11eb-81f4-e80f20c52789.png)
User에 담길 DB 설정.

  > ### 실행 화면
![image](https://user-images.githubusercontent.com/70474860/121338152-31846f80-c958-11eb-8414-a07b6b1c329e.png)
#### 입력값 넣기
![image](https://user-images.githubusercontent.com/70474860/121338265-4e20a780-c958-11eb-9d7e-d6421cc227f3.png)
#### Firebase Authentication에 입력한 내용이 저장된 모습
![image](https://user-images.githubusercontent.com/70474860/121338439-78726500-c958-11eb-841f-2fa02f8c0f9c.png)
#### Firebase Realtime Database에 입력한 내용(이메일, 닉네임, 비밀번호)이 저장된 모습

### 2.로그인
  
  로그인의 주요 기능은 다음과 같다.

    1) 로그인 형식 설정(입력창 빈칸이 있는지, 이메일/비밀번호 형식을 지켰는지, 상명대 웹메일 도메인 주소를 사용했는지 등)
    2) 입력한 이메일과 비밀번호가 Firebase Authentication의 계정과 동일한지에 대한 여부
    4) 인증하지 않은 이메일일 경우 한 번 더 해당하는 이메일에 메일 전송

사용 파일 :

    - activity_login.xml : 로그인 레이아웃 작성
    - LoginActivity.java : 로그인을 실행하는 코드 작성
  
  > ### activity_login.xml
  #### 레이아웃 화면
![image](https://user-images.githubusercontent.com/70474860/121340102-2a5e6100-c95a-11eb-83bb-745107dfec72.png)
   
  #### 코드
![image](https://user-images.githubusercontent.com/70474860/121339827-d94e6d00-c959-11eb-848d-18237096c86a.png)

  > ### LoginActivity.java
  
  #### import 및 변수 선언
![image](https://user-images.githubusercontent.com/70474860/121340238-55e14b80-c95a-11eb-8f46-653343620638.png)

  #### onCreate() 메소드
![image](https://user-images.githubusercontent.com/70474860/121340288-698cb200-c95a-11eb-8d2a-d042c648eeb6.png)
activity_login.xml에 있는 객체들을 변수와 link.
OnClickListener 호출.
  btnRegister 버튼 클릭 시 RegisterActivity.java 클래스 호출 -> 회원가입 layout 장면 전환.
  btnLogin 버튼 클릭 시 userLogin() 메소드 호출
  
   #### userLogin() 메소드
  ![image](https://user-images.githubusercontent.com/70474860/121340985-2da61c80-c95b-11eb-93e1-9896d9c0c074.png)
이메일 입력을 안 했을 때, 비밀번호 입력을 안 했을 때, 이메일 형식이 아닐 때, 상명대 웹메일 도메인 주소가 아닐 때, IsValidPwd()==false일 때 에러 메시지 출력.
정상적으로 입력했을 때, Authentication의 DB와 이메일, 비밀번호 입력값을 비교 후 존재하는 계정이면 로그인 성공 후, MainActivity로 이동. 이때, 이메일 인증이 완료되지 않은 계정이면 다시 메일을 전송 후 이메일 인증을 완료하라는 에러 메시지 출력.
존재하지 않는 계정이면 로그인 실패 에러 메시지 출력.

   #### IsValidPwd() 메소드
![image](https://user-images.githubusercontent.com/70474860/121341800-fc7a1c00-c95b-11eb-87c7-87c7ff0dd884.png)
비밀번호 형식이 맞으면 true 값 반환. 아닐 경우, false 반환.

  > ### 실행 화면
![image](https://user-images.githubusercontent.com/70474860/121342120-4fec6a00-c95c-11eb-92bd-530d310b6530.png)
#### 이메일 인증을 완료하지 않아 에러메시지가 출력되는 모습.
![image](https://user-images.githubusercontent.com/70474860/121342293-74e0dd00-c95c-11eb-9080-b98825c27a6d.png)
#### 해당 이메일로 메일이 전송된 모습.
![image](https://user-images.githubusercontent.com/70474860/121342446-980b8c80-c95c-11eb-8c93-624670d5038d.png)
#### 메일에 있는 링크를 통해 이메일 인증을 완료한 모습.

  ## - 메인화면(양하은)
  
  ![image](https://user-images.githubusercontent.com/80022793/121352881-41a44b00-c968-11eb-96ce-7ef54c602d25.png)
  ![image](https://user-images.githubusercontent.com/80022793/121352847-39e4a680-c968-11eb-8f3e-cc7caf8dc7fb.png)

  메인화면의 주요기능은 페이지 이동이다. 
  Fragment를 이용하여 bottomNavi를 제작하여 구성하려고 하였으나 4번째 화면을 수업시간에 배운 내용을 응용하여 CardView로 교통부분의 페이지를 구성하였는데,
  이 부분과 충돌이 일어나 이를 해결하지 못하고 버튼을 이용하여 다른 화면으로 넘어 가도록 구성하였다. 
  
  ![image](https://user-images.githubusercontent.com/80017979/121243436-ac0daa80-c8d8-11eb-94cd-65a55e6a1212.png)
  
  
  ![image](https://user-images.githubusercontent.com/80022793/121352865-3d782d80-c968-11eb-974b-c68c7e643f94.png)
  
  또한 DrawerListener를 사용하여 모든 기능으로 연결될 수 있도록 버튼을 넣어 이동의 편리함을 추가하였다.  
  
  ![image](https://user-images.githubusercontent.com/80017979/121243464-b3cd4f00-c8d8-11eb-9a72-d8d410f54afd.png)
  ![image](https://user-images.githubusercontent.com/80017979/121243473-b62fa900-c8d8-11eb-8d37-3c8650e5b33a.png)
  
  뿐만 아니라 상명대학교 학생들의 편리함을 위해 제작된 만큼 학생들이 가장 많이 사용하는 학교 홈페이지, e-campus, 홈페이지 중에서도 공지사항의 url을 버튼에 intent로 연결하여 학교와의 정보공유를 쉽게 할 수 있도록 하였다.
  
  
  ![image](https://user-images.githubusercontent.com/80022793/121352921-4bc64980-c968-11eb-8d89-5dfe15f39846.png)
  ![image](https://user-images.githubusercontent.com/80022793/121352944-51bc2a80-c968-11eb-86c7-3f287ec6dd85.png)
  ![image](https://user-images.githubusercontent.com/80022793/121352960-54b71b00-c968-11eb-9c0e-6661642770f0.png)
  
  ![image](https://user-images.githubusercontent.com/80017979/121243561-cd6e9680-c8d8-11eb-86f9-ce3690db2da7.png)
  </details>
  
<details>
  <summary> - 게시판(심우정)</summary>
  
    - 게시판의 세부 기능
    1.게시글 작성, 불러오기
    2.게시글과 댓글 작성시 익명기능
    3.게시글 사진 첨부 기능
    4.각 게시글 댓글 작성 기능
    5.각 게시글 좋아요 기능

### 게시판의 각 기능 구현 방법

  > ### 1.게시글 작성, 불러오기

게시판 레이아웃은 RecyclerView와 Adapter를 이용해 작성하였다.
그리고 게시판을 실시간으로 게시글을 작성하고, 불러올 수 있어야 한다고 생각해서 DB 저장과 호출을 실시간으로 사용할 수 있는 Firebase의 RealtimeDatabae를 활용했다.
RealtimeDatabase를 사용하기 위해서는 Firebase 페이지에서 새 프로젝트를 만들어 먼저 안드로이드 스튜디오를 연동한다. 그 후 안드로이드 스튜디오 내에서 코드를 이용해 RealtimeDatabase를 연동해 게시글을 작성하면 Board테이블에 글에 대한 DB가 들어가고, 게시판 목록 리사이클러뷰에 게시글 목록을 역순으로 불러오도록 코드를 작성했다. 자세한 코드 내용은 안드로이드 스튜디오 코드에 주석처리 하였다.

<img width="800px" height="500px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225231-457e9180-c8c4-11eb-8060-c9a232c8aaab.png"><br/>
<img width="800px" height="500px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225250-4adbdc00-c8c4-11eb-9ff6-28c84adebac5.png"><br/>
**(WriteActivity.java, 게시글 작성하는 코드)**<br/>

<img width="800px" height="500px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225262-50392680-c8c4-11eb-9a7a-eef0721c61db.png"><br/>
**(BoardActivity.java, 게시글 DB를 가져와 배열리스트에 넣고 리사이클러뷰(게시글 목록)로 보내주는 코드)**<br/>
<br/>
  > ### 2.게시글과 댓글 작성시 익명기능

게시글이나 댓글을 작성할 때 익명으로 글/댓글을 작성할 건지, 닉네임이 나타나게 글/댓글을 작성할 건지 체크박스를 통해 입력을 받아 처리할 수 있도록 기능을 구현했다.
Activity클래스에서 레이아웃의 체크박스를 findViewid해서 가져와<br/>
```
if (checkBox_b.isChecked()) {
result = "익명"; // 글을 쓸 때 익명 체크박스를 체크하면 익명으로 저장
} else {
result = name; // 글을 쓸 때 익명 체크박스를 체크하지 않으면 닉네임으로 저장
}
```
isChecked()로 체크 여부에 따라 작성자를 익명인지, 닉네임인지를 result 변수에 저장하고, 그 변수를 다른 글/댓글 데이터와 함께 Firebase DB로 보낸다. 그 후에 글/댓글을 받아와 출력해 보여줄 때는 그냥 테이 블 내의 result를 받아와 보여주면 되도록 구현했다.
 
<img width="740px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225321-5f1fd900-c8c4-11eb-91ad-1386aa59439c.png"><br/>
**(CommentActivity.java, 댓글 작성하는 코드 String result_c=“”; result_c 변수를 하나 만들고, 댓글 옆에 익명 체크박스 여부에 따라 각각 익명인지, 닉네임인지 할당하고, 그걸 CommentInfo 배열리스트에 담아 RealtimeDatabase의 Comment라는 DB 테이블에 보냄, 게시글 작성하는 WriteActivity도 동일. 1번 사진 참고)**
<br/>
<br/>
  > ### 3.게시글 사진 첨부 기능

Firebase의 RealtimeDatabse나 FireStore은 이미지 파일은 담지 못 한다. 그래서 이미지 파일이나 동영상 파일을 저장하기 위해서는 storage를 활용해야만 했다.
RealtimeDatabase처럼 Firebase 사이트에서 Storage를 시작하고 프로젝트 권한을 읽고 쓸 수 있도록 권한을 수정해준다. 그 후 안드로이드 스튜디오 내에 스토리지 라이브러리를 implement로 넣어주면 스토리지에 파일을 넣고 불러올 준비는 끝난다.
먼저 이미지 파일을 넣는 작업은 게시글을 작성하는 클래스, WriteActivity에서 일어난다. storage 인스턴스를 만들고, 참조한다. 그 후 파일을 저장할 때의 파일명을 만들어주고, storage 폴더 경로에 접근해 게시글 사진을 저장할 경로 지정해준다. 그리고 그 경로에 이미지 파일을 저장해준다.
 
<img width="800px" height="170px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225373-6ba43180-c8c4-11eb-865e-bae8254e61f3.png"><br/>
<img width="800px" height="270px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225382-6e068b80-c8c4-11eb-9f39-c79c4d97d852.png"><br/>
**(WriteActivity.java)**<br/><br/>
나는 여기서 각 게시글에 이미지가 첨부됐는지, 안 됐는지, 첨부됐다면 몇 번 글에 어떤 이미지가 들어갔는지 구분하기 위해 이미지 파일 명을 board_num(각 게시글 번호)+”번째 글 사진.png”로 파일명을 지정해주었다. 그리고 filePath != null이면 이미지 파일을 첨부한다는 조건으로, int image변수를 하나 만들어 첨부하면 1, 첨부하지 않으면 0으로 할당해, 게시글 DB 테이블에 글에 대한 정보로 같이 저장하였다.
그 후에 게시글을 클릭해서 게시글에 대한 정보와, 이미지 파일을 불러올 때 board_num의 child인 image가 0인지, 1인지에 따라 이미지가 첨부된 글인지 판단할 수 있도록 했다. 여기서 만약 이미지가 첨부된 게시글을 클릭해, 이미지 파일을 불러올 경우를 보겠다.
<br/>
<img width="800px" height="200px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225422-778ff380-c8c4-11eb-9e2f-1391c4100ff2.png"><br/>
**(BoardImageActivity.java)**<br/><br/>
이미지가 첨부된 게시글에 대한 데이터를 가져와 보여주는 BoardImageActivity에서 보낼 때와 마찬가지처럼 먼저 storage인스턴스를 만들고, 참조한다. 그 다음 내가 찾기 위한 사진이 있는 경로를 찾아준다. 나는 storage파일/images/board_num+”번째 글 사진.png” 경로이기 때문에 storageRef.child("images/"+board_num+"번째 글 사진.png”)라고 작성했고, 이 뒤에 바로 getDownloadUrl().addOnSuccessListener(new OnSuccessListener<Uri>() 라고 Url을 받아오는 코드를 작성해준다. 이미지 파일의 url을 받아오는데 성공(onSuccess)하면<br/>
Glide.with(getApplicationContext()).load(uri).into(board_image);<br/>
Glide라이브러리를 이용해 값을 받아 ImageView에 해당 이미지 uri값을 할당해서 넣어주면 된다.
<br/><br/>
   
  > ### 4.각 게시글 댓글 작성
  
게시판의 게시글 목록에서 클릭한 게시글의 내용을 불러와 보여주는 것까지 구현한 후에, 각 게시글의 댓글을 달 수 있는 기능을 추가하기 시작했다. 사실 게시글 구현한 방식과 거의 유사하다. 게시글을 보여주는 레이아웃에 게시글 목록을 보여주는 RecyclerView처럼 댓글 목록을 RecyclerView로 구성했다. 대신 게시글 목록을 보여주는 RecyclerView는 한 화면을 꽉 채웠다면 댓글 목록을 보여주는 화면에는 그 댓글을 단 게시글 내용도 같이 보여주어야 하기 때문에 게시글 내용 밑에 작게 recyclerview를 구성해놓았다. 또 게시글을 작성할 땐 글작성 버튼을 먼저 누르고, Intent로 글작성 레이아웃에서 작성한 후 DB로 보냈다면, 댓글을 DB를 보내고 불러오는 과정이 한 화면에서 이루어져야하기 때문에 CommentActivity(이미지 첨부 없는 게시글), BoardImageActivity(이미지 첨부 있는 게시글)내에서만 코드가 이뤄진다.
 
<img width="740px" height="120px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225455-7f4f9800-c8c4-11eb-9d43-2a81fe60d712.png"><br/>
**(WriteActivity.java, board_num 저장하는 부분)**<br/>
 
<img width="740px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225477-84ace280-c8c4-11eb-85c1-d3c4d9af4bf5.png"><br/>
**(CommentActivity.java, 댓글 작성해 DB 보내는 부분)**<br/>
 
<img width="740px" height="140px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225493-89719680-c8c4-11eb-90da-363a2d27d37f.png"><br/>
**(CommentActivity.java, BoardAdapter에서 몇 번 글을 클릭했는지, 그 글의 데이터와 함께 intent로 쏴준 걸 받아온다.)**<br/><br/>
그리고 댓글을 작성하고 불러오는데에는 큰 어려움 없이 구현했지만, 특정 게시글에 대한 댓글을 골라 불러오는데 몇 일 어려움을 겪었다. 파이어베이스는 mySql처럼 DB를 추가할 때마다 수가 늘어나는 auto_increment기능이 없기 때문에 어떻게 board_num이라는 변수를 만들어 수를 계속 늘릴 수 있을까 고민했다. 그냥 DB를 보낼 때마다 +1을 하면 board_num이 계속 1로만 들어가고, 점점 늘리려면 전 글의 board_num을 불러오고 또 +1을 한 다음에 새로운 다음 글 테이블을 만들 때 넣어야 하기 때문에 코드가 쓸데없이 복잡해지고, 비효율적이기 때문에 다른 방법을 고안해냈다.
내가 게시글을 RecyclerView를 통해 각 게시글을 item레이아웃으로 나타내는데, 이 아이템 갯수를 받아올 수 있는 코드가 있었다. RecyclerView에 대한 코드들은 어댑터에서 이뤄지기 때문에
BoardAdapter.getCount(); 이렇게 작성하면 게시글 목록의 개수를 구해올 수 있었다.
즉 글을 새로 작성할 때 저 코드를 통해 작성된 게시글 개수를 받아와서 +1을 해준 다음 board_num에 넣어주고 DB로 보낸다면 각 글에 맞는 board_num을 게시글 테이블에 넣어주는 것이다.
 
<img width="740px" height="170px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225521-8f677780-c8c4-11eb-9a62-b22def1ac514.png"><br/>
**(CommentActivity.java)**<br/><br/>
  
그 후에 댓글을 불러올 때 myRef_c.orderByChild("board_num").equalTo(board_num)
Firebase에서 댓글 DB를 저장한 Comment테이블을 참조(myRef_c)하고 orderByChild를 사용하여 “board_num”이 포함된 데이터를 가져온다. 그 다음 “board_num”의 값이 각 게시글에 맞는 번호 board_num인 댓글 데이터를 가져오는 방식이다.
Ex) 게시글 목록에서 1번 글을 클릭해 1번 글에 대한 내용을 보여주는 레이아웃에서 댓글을 달면 댓글 데이터는 board_num=1로 저장이 된다. 그 후에 1번 글을 보면 board_num을 가지고 있는 댓글 데이터들 중에 board_num이 1인 댓글 데이터를 불러오는 것

<img width="300px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225557-99897600-c8c4-11eb-83de-cd2e700a22d4.png"><img width="300px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225575-9ee6c080-c8c4-11eb-8458-0ff958f1bd00.png"><br/>
  **(RealtimeDatabase 테이블, 저 1번글, 2번글에 있는 1,2,3.. 숫자가 board_num, 1번글 안에 있는 board_num은 각 글에 맞는 번호 저장한 것)**<br/>
  <br/>

  > ### 5.각 게시글 좋아요 기능
  
게시글 좋아요 기능도 위에 댓글 구현 방법과 유사하다. board_num을 통해 게시글을 구분해 DB를 넣고 불러오는데, 댓글 구현 방법과 다른 점은 DB를 보내는 코드와 받아오는 코드가 따로따로 작성되는 댓글과 달리, 좋아요는 DB를 불러오는 코드 안에 DB를 보내는 코드가 작성된다.
먼저 Like 테이블을 참조해 데이터를 가져오는데, 조건을 걸고 가져온다. Like테이블의 자식 board_num번글, 또 그의 자식인 like_count를 가져와서 좋아요 개수에 따라 클릭하면 늘리거나, 줄이거나하고 다시 DB에 넣어주는 코드를 작성한다. 대신 클릭할 때마다 board_num번글 좋아요에 대한 테이블을 지워준다. 지워주지 않으면 like_count가 바뀔 때마다 새로운 테이블이 생성되게 되기 때문에 꼭 지워줘야 한다.

<img width="740px" height="450px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225605-a73efb80-c8c4-11eb-826d-f2f565ebd336.png"><br/>
<img width="740px" height="450px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225627-ad34dc80-c8c4-11eb-8fdf-260e76574806.png"><br/>
**(CommentActivity.java, 좋아요 버튼 이벤트 코드)**<br/>
 
<img width="300px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225654-b32abd80-c8c4-11eb-8239-d1f47a9cd7a2.png"><br/>
**(RealtimeDatabase의 Like(좋아요)테이블, 이 상태에서 2번 글의 좋아요를 누르면 2번 글의 like_count가 1이 되고, 1번 글의 좋아요를 취소하면 1번 글의 like_count가 0이 된다.)**<br/><br/>
  </details>

<details>
  <summary> - 시간표 / 일정 추천(박윤빈)</summary>
  
  > ### 1. AutoResizeTextView.class
  
  테이블 안에 칸 하나의 크기의 가로 축과 세로 축 크기를 맞추기 위해서 자동 조절이 가능한 기능을 추가하였다. 가로축 및 세로축을 균일하게 조정하기 위해서 AUTO_SIZE_TEXT_TYPE_UNIFORM을 지정했다. 
  
  ![1](https://user-images.githubusercontent.com/79950206/121289754-6f65a180-c920-11eb-9c18-aeb5ede8005e.jpg)
  ![3](https://user-images.githubusercontent.com/79950206/121289768-74c2ec00-c920-11eb-8761-998158c64dd7.jpg)
  
  > ### 2. Course.class (마혜준/박윤빈)
  
  상명대학교 강의를 불러오기 위해서 학교 홈페이지에 있는 강의를 불러와 내가 필요한 정보를 속성 courseID	courseYear	courseTerm	courseArea	courseMajor	courseGrade	courseTitle	courseCredit	courseDivide	courseProfessor	courseTime 으로 지정하고 정렬했다. 2번째 사진은 이렇게 정리한 csv 파일을 json 파일로 전환시켰다. json 파일을 통해서 안드로이드 스튜디오에서 불러오기 위해서이다. 3번째는 내가 만든 속성에 getter setter 메소드를 추가한 class이다.

  ![image](https://user-images.githubusercontent.com/79950206/121291347-1a775a80-c923-11eb-931d-1cd74c777d4c.png)
  ![image](https://user-images.githubusercontent.com/79950206/121291384-282ce000-c923-11eb-83a7-88f37b60af30.png)
  ![6](https://user-images.githubusercontent.com/79950206/121290874-51993c00-c922-11eb-99af-ced3e1d16309.jpg)

  > ### 3. CourseActivity.class
  
  CourseActivity에는 년도, 학기, 전공/교양, 학과를 yearSpinner, termSpinner, areaSpinner, majorSpinner로 설정해 Spinner로 구성하였다. arrays에 각각의 Spinner에 들어가서 보여질 정보를 입력해두었고 Spinner를 누르면 내가 arrays에 넣어둔 정보를 선택해 클릭할 수 있다. 4개의 Spinner 중 하나라도 선택되지 않으면 정보를 볼 수 없도록 각각의 Spinner가 선택되지 않았을 때 "해당 년도를 입력하세요.", "해당 학기를 입력하세요.", "전공을 입력하세요.", "학과를 입력하세요." 와 같은 문구가 밑부분에 떴다가 사라지도록 설정하였다. 
  
  ![7](https://user-images.githubusercontent.com/79950206/121293089-02eda100-c926-11eb-8200-0dab883385e6.jpg)
  ![8](https://user-images.githubusercontent.com/79950206/121293172-257fba00-c926-11eb-84ba-677f7f0f1a1e.jpg)
  
  > ### 4. TimetableActivity.class
  
  TimetableActivity는 MainActivity와 같은 역할이다. 각각의 프로젝트를 합치는 과정에서 같은 이름의 Activity가 겹치는 것을 막기 위해서 TimetableActivity로 이름을 바꾸어 작업하였다. TimetableActivity에 버튼 3개를 두어 scheduleButton을 누르면 시간표 activity로 이동하고, courseButton을 누르면 강의 목록을 볼 수 있는 activity로, calendarButton을 누르면 달력을 볼 수 있는 페이지로 이동하도록 설정하였다. calendar가 보이는 화면은 activity가 아니라 fragment를 이용해서 구현했기 때문에 activity와는 다르게 FragmentManager fragmentManager = getSupportFragmentManager(); / FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction(); / fragmentTransaction.replace(R.id.container, new CalendarFragment()); / fragmentTransaction.commit(); 구현하였다.
  
  ![image](https://user-images.githubusercontent.com/79950206/121294031-a8eddb00-c927-11eb-9712-5c3029e7c7d0.png)
  
  > ### 5. CalendarFragment.class
  이 부분은 일정 추천부분이다. 원래 구현하고자 했던 일정 추천은 팀플과 같은 여러 사람이 시간을 맞춰야 하는 모임에서 각자의 시간표에 비어있는 시간대에 일정을 추천해주는 기능이었다. 하지만 강의 목록에서 강의도 제대로 불러오지 못하는 과정에서 원래의 일정 추천을 실행하는 것이 무리라고 생각되어 캘린더를 띄우고 원하는 날짜로 이동해 오늘 할 일을 적어 저장해두는 To-Do List 역할을 하도록 변경하였다. 캘린더는 안드로이드 스튜디오에서 제공되는 calendarView를 이용했다. 날짜가 변경될 때 이벤트를 받기 위한 리스너는 void setOnDateChangeListener(CalendarView.OnDateChangeListener listener) 이고 void onSelectedDayChange(CalendarView view, int year, int month, int dayOfMonth) 이 메소드를 이용해 선택된 날짜의 정보를 얻었다.

  ![10](https://user-images.githubusercontent.com/79950206/121300910-654c9e80-c932-11eb-92ad-9add3f15ba2e.jpg)
  ![11](https://user-images.githubusercontent.com/79950206/121300985-82816d00-c932-11eb-8f66-21a15a545067.jpg)
</details>

<details>
  <summary> - 친구목록/채팅(양하은)</summary>
  
  메인화면에서 친구목록 버튼을 눌러 이동하면 친구를 추가 할 수 있는 화면이 나온다. 그리고 목록에 추가된 친구를 누르면 해당 친구와의 채팅이 가능한 화면이 나온다
  
  ![image](https://user-images.githubusercontent.com/80022793/121352999-5c76bf80-c968-11eb-800b-27d88e0e4375.png)
  
  ![image](https://user-images.githubusercontent.com/80022793/121353083-70babc80-c968-11eb-87e0-a9888f076c30.png)
  ![image](https://user-images.githubusercontent.com/80022793/121353101-757f7080-c968-11eb-99a4-9430552fe496.png)
  
  ListView를 사용하여 구성하였고, 버튼에 이벤트를 넣어 닉네임을 추가할 수 있도록 하였으며 Adapter를 이용하여 불러오기를 하였다 
  
  채팅의 주요기능은 실시간으로 대화를 할 수 있다는 것이다. 
  
  ![image](https://user-images.githubusercontent.com/80022793/121353031-64366400-c968-11eb-8a17-7ed640994683.png)
  
  ![image](https://user-images.githubusercontent.com/80017979/121243122-4f11f480-c8d8-11eb-8e60-4117003bff26.png)
  ![image](https://user-images.githubusercontent.com/80017979/121243196-694bd280-c8d8-11eb-83a9-0bc1e3e1301b.png)
  ![image](https://user-images.githubusercontent.com/80017979/121243210-6b159600-c8d8-11eb-80c7-ef323e30b803.png)

  Firebase의 Realtime database를 이용하여 제작하였다. ChatActivity를 중심으로 ChatAdapter를 이용하여 firebase에 전송하고 받아오도록 하였다. 
  뿐만 아니라 닉네임을 설정할 수 있으며 설정한 닉네임으로 채팅을 보낸다.
  
  ![image](https://user-images.githubusercontent.com/80017979/121243227-723ca400-c8d8-11eb-996a-461099ab5a05.png)

  실시간으로 채팅을 주고받을 수 있으며 addChildEventListener를 이용하여 채팅을 주고받는 것을 구현하였다. 
  원래는 단체채팅을 가능하게 하는 것이 목표였지만 채팅을 하는 채팅방안에 2명 이상의 유저를 등록하는 것을 실패하여 결국 구현하지 못하였다
  
  ## - 셔틀버스시간표(김다혜)
  셔틀버스 시간표에서 가장 중요한 기능은 실시간 타이머를 구현하는 것이었다. 
  
  ![image](https://user-images.githubusercontent.com/80017979/121248102-fb0a0e80-c8dd-11eb-9388-2da9b62b0079.png)
  
  getTime()함수는 현재 시간을 년, 월, 일, 시, 분. 초로, 셔틀버스 시, 분을 순서대로 배열에 저장한다. 
  저장된 배열의 크기만큼 if문을 이용하여 차례대로 셔틀버스 시간에서 현재 시간을 빼 셔틀버스가 출발하기까지 남은 시간을 계산한다. 계산결과가 음수가 된다면 현재 시간에서 하루를 더한 후 다시 계산한다.
  계산결과를 다시 String 배열에 저장하고 그 값을 return해준다. getTime()함수는 두정역에서 출발하는 셔틀버스의 남은 시간을 계산하는 getTime1()과, 
  학교에서 출발하는 셔틀버스의 시간을 계산하는 getTime2()로 나뉘어져 있다.
  
  ![image](https://user-images.githubusercontent.com/80017979/121248162-0f4e0b80-c8de-11eb-8abd-b1b6ad94effb.png)
  ![image](https://user-images.githubusercontent.com/80017979/121248169-11b06580-c8de-11eb-91cb-e634e3e2be25.png)
  
  레이아웃은 spinner를 이용해 선택된 아이템에 따라 텍스트를 변경할 수 있도록 구현하였다. 
  spinner와 아이템 값을 저장한 list를 연결한 후 setOnItemSelectedListener를 이용해 선택된 아이템에 따라 텍스트 뷰의 텍스트를 변경하고, 타이머를 실행시킨다. 
  초기에는 타이머를 시작하는 코드만 작성하였으나, 자꾸 두 개의 타이머가 충돌하는 문제가 발생하여 하나의 아이템을 선택하면 다른 아이템에 연결된 타이머를 정지시켜준 후에 선택된 타이머를 실행시키도록 구현하였다.
</details>

<details>
  <summary> - 버스시간표(박승준)</summary>
  
  > ### 1. RetrofitManager
  Retrofit은 안드로이드 애플리케이션에서 통신 기능에 사용하는 코드를 사용하기 쉽게 만들어 놓은 라이브러리이다.
  데이터를 보다 쉽게 가져오고 업로드 할 수 있게 한다.
  먼저 ApiUrl에 api 주소를 입력한다.
  Retrofit에 초기 생성문을 작성해 하나의 변수로 만들어 api 주소를 불러 올 수 있게 한다.
  
![image](https://user-images.githubusercontent.com/80312446/121250816-20e4e280-c8e1-11eb-895a-e2dff0196c77.png)
 
  > ### 2. Service
  RetrofitService interface를 작성한다.
  GET 타입으로 각각의 Query에 정보(url)를 입력한다.
  Header는 보안을 위해 Query대신 사용했다.
  
  ![image](https://user-images.githubusercontent.com/80312446/121250977-52f64480-c8e1-11eb-9c4a-7af416024f4d.png)

  > ### 3. BusMainActivity
  api를 사용하기 위해서 URLDecode를 사용해야 한다. 이를 위해서는 인증이 필요하지만 공공데이터포털의 인증이 이루어지지 않았고,
  문의를 남겨보았지만 답변이 오지 않았다.
  하여 loadRealBusInfo 사용 대신 loadPostmanBusInfo를 사용했다.
  그리고 getPostmanBusInfo의 serviceKey를 받는다.
  busStopNumber로 검색하기 버튼을 클릭하면 text의 값을 확인해준다.
  text가 비었으면 정보를 바르게 입력하도록 출력하고 정보가 있다면 해당 정보의 api를 불러온다.
  city코드는 천안에 해당하는 25를 입력했다.
  서버에서 반환해준 callback으로 interface를 object형식으로 override 받는다.
  onResponse와 onFailure를 이용해 올바른 정보출력 형식인지 여부를 판단한다.
  response.body가 response 값을 받아온다. 
  
![image](https://user-images.githubusercontent.com/80312446/121251442-d57f0400-c8e1-11eb-9c84-3c765a49ed76.png)
![image](https://user-images.githubusercontent.com/80312446/121251485-e3cd2000-c8e1-11eb-95cd-9f66febf8991.png)

  > ### 4. Bus
  response값을 받아오면 Bus로 형을 변환해준다.
  그러면 data를 타게 되고 Array 형식으로 BusModel 값들을 불러온다.
  
  ![image](https://user-images.githubusercontent.com/80312446/121251585-019a8500-c8e2-11eb-98c4-9198876f7fb1.png)

  > ### 5. BusModel
  Model에서 정류소 값을 받았을 경우 출력되는 차량 번호, 남은 구간, 남은 시간을 출력해준다.
  그 다음은 intent를 이용하여 Busdetail을 불러온다.
  startActivity에서 BusDetailActivity로 넘어간다.
  
  ![image](https://user-images.githubusercontent.com/80312446/121251682-1c6cf980-c8e2-11eb-8784-2adc93c113c3.png)

  > ### 6. BusDetailActivity
  BusDetailActivity로 넘어오면 getSerializableExtra로 정보를 받아온다.
  lazy(지연초기화)를 사용하여 데이터 호출시에 데이터를 즉시 초기화 시켜서 온다.
  그 후 RecyclerView로 넘긴다.
  
  ![image](https://user-images.githubusercontent.com/80312446/121251756-2f7fc980-c8e2-11eb-9e08-cfef3734a993.png)

  > ### 7. RecyclerView
  RecyclerView를 이용하여 데이터를 효율적으로 표시 할 수 있도록 한다.

![image](https://user-images.githubusercontent.com/80312446/121251836-44f4f380-c8e2-11eb-8fd8-0b99744ccdb5.png)

  </details>
  
<details>
  <summary> - 택시 모집(김다혜)</summary>
  
  택시 모집은 택시의 출발장소, 도착장소, 탑승인원, 탑승시간을 입력하여 글을 작성하면 그 글을 통해서 같이 택시 탈 사람을 모집하는 기능이다. 택시 모집의 레이아웃은 recyclerview를 이용했다. 또한 게시글을 작성하면 firebase의 realtimedatabase에 정보를 저장하고 recyclerview에 게시글을 불러오게 된다. 
  
  ![image](https://user-images.githubusercontent.com/80017979/121305951-32f26f80-c939-11eb-9a0d-9a12a922a5c8.png)
  ![image](https://user-images.githubusercontent.com/80017979/121306001-47366c80-c939-11eb-9028-1cdcb41534e9.png)
  ![image](https://user-images.githubusercontent.com/80017979/121277296-45ee4b00-c90b-11eb-95db-281e4be345a2.png)

  게시글을 내용을 db에 저장하는 코드이다. db에 정보를 넣어주기 위해서 Board라는 클래스를 만들었다. 버튼을 클릭하면 EditText에 값이 들어와 있는지 확인한 후 모두 값이 들어와 있다면 들어있는 텍스트값을 읽어와서 Board에 넣어준 후 TaxiBoard테이블에 board값을 넣어준다. 
  
  ![image](https://user-images.githubusercontent.com/80017979/121277318-4e468600-c90b-11eb-9a35-91d421c68944.png)
  
  택시 장소를 입력할 때 카카오맵 api를 이용하여 지도를 띄우는 것은 성공했지만, 클릭리스너에서 발생하는 오류를 해결하지 못해 좌표 값을 받아오는 것은 구현하지 못했다.
  
  ![image](https://user-images.githubusercontent.com/80017979/121277357-674f3700-c90b-11eb-8652-ac40267f618f.png)
  ![image](https://user-images.githubusercontent.com/80017979/121277379-733af900-c90b-11eb-804c-72710920e9e6.png)

  이렇게 저장된 데이터는 다시 firebase에 연결하여 가져오게 된다. 이 과정에서 CustomAdapter를 이용하여 recyclerview에 item을 생성하여 게시판을 만들게 된다.
  
  ![image](https://user-images.githubusercontent.com/80017979/121306259-92e91600-c939-11eb-8371-209c89029a8a.png)
  ![image](https://user-images.githubusercontent.com/80017979/121306528-e9eeeb00-c939-11eb-812c-db6f78e0b81b.png)

  
  또한 item마다 onClick을 연결하여 item이 클릭될 때 마다 인원이 1명씩 증가한다. 원래는 db에 있는 값을 수정해서 숫자를 늘리고 싶었지만 키값을 받아오는 것이 잘 해결되지 않아 새로 값을 추가하도록 구현하였다. 현재 인원과 모집인원이 같아지게 되면 Toast를 통해 인원이 가득찼다는 메시지와 함께 더 이상 인원이 늘어나지 않는다.
</details>
  
# SMUtime의 차별점
  
  SMUtime은 다른 어플들과는 다르게 상명대학교 학생들만을 위한 어플이다. 따라서 상명대학교 학생들을 위한 차별화된 편의 기능을 제공하고 있다. 
  
  회원가입을 할 때 상명대학교 이메일을 사용해야만 가입할 수 있다. 
  
  SMUtime은 firebase를 이용한 기능들이 많이 사용되고 있는데 firebase를 이용한 SMUtime만의 차별점은 첫번째로 실시간 채팅이다. 실시간 채팅으로 같은 학교에 다니는 사람들 끼리 서로 메세지를 주고 받고, 정보를 공유 할 수 있다. 
  
  두번째로 셔틀버스 시간표를 제공하고 있다. 학교에서 제공된 셔틀버스 시간을 이용하여 셔틀버스 출발까지 남은 시간을 알려준다. 
  
  세번째로 상명대학교 근처에 있는 버스정류장에 대한 버스 도착정보를 제공하고 있다. 학교 주변에 있는 안서동보A, 상명대학교 정류장의 정류장 번호 또는 정류장 이름을 입력하면 버스들의 도착정보를 제공한다. 
  
  네번째로 택시 모집 기능을 제공하고 있다. 택시 모집기능은 출발 장소, 도착 장소, 출발 시간 등을 올리면 그 정보를 확인하고 같이 택시를 타고 싶은 사람들이 모일 수 있도록 한다.'

  
# 최종 프로젝트
  https://github.com/mhjoon99/SMU-Time
  
# 실행 영상
  https://youtu.be/kbgkV3GHTAU
  - 제작: 양하은, 심우정
  

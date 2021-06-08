# SMUtime

  SMUtime은 상명대학교 학생들을 위한 편의기능을 제공하는 커뮤니티이다.
  
## SMUtime의 주요 기능

    - 회원가입/로그인
    - 게시판
    - 시간표
    - 채팅
    - 셔틀버스시간표
    - 버스시간표
    - 택시 모집

## SMUtime 구성도
![화면 구성](https://user-images.githubusercontent.com/80017979/121213328-17478480-c8b9-11eb-8e22-ff5b9b712788.jpg)

## 기능 구현

  -회원가입/로그인
  
  ---
  -게시판(심우정)
  
    - 게시판의 세부 기능
    1.게시글 작성, 불러오기
    2.시글과 댓글 작성시 익명기능
    3.게시글 사진 첨부 기능
    4.각 게시글 댓글 작성 기능
    5.각 게시글 좋아요 기능

### 게시판의 각 기능 구현 방법

  > #### 1.게시글 작성, 불러오기

게시판 레이아웃은 RecyclerView와 Adapter를 이용해 작성하였다.
그리고 게시판을 실시간으로 게시글을 작성하고, 불러올 수 있어야 한다고 생각해서 DB 저장과 호출을 실시간으로 사용할 수 있는 Firebase의 RealtimeDatabae를 활용했다.
RealtimeDatabase를 사용하기 위해서는 Firebase 페이지에서 새 프로젝트를 만들어 먼저 안드로이드 스튜디오를 연동한다. 그 후 안드로이드 스튜디오 내에서 코드를 이용해 RealtimeDatabase를 연동해 게시글을   작성하면 Board테이블에 글에 대한 DB가 들어가고, 게시판 목록 리사이클러뷰에 게시글 목록을 역순으로 불러오도록 코드를 작성했다. 자세한 코드 내용은 안드로이드 스튜디오 코드에 주석처리 하였다.

<img width="800px" height="500px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225231-457e9180-c8c4-11eb-8060-c9a232c8aaab.png"><br/>
<img width="800px" height="500px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225250-4adbdc00-c8c4-11eb-9ff6-28c84adebac5.png"><br/>
(WriteActivity.java, 게시글 작성하는 코드)<br/>

<img width="800px" height="500px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225262-50392680-c8c4-11eb-9a7a-eef0721c61db.png"><br/>
(BoardActivity.java, 게시글 DB를 가져와 배열리스트에 넣고 리사이클러뷰(게시글 목록)로 보내주는 코드)<br/>
<br/>
  > #### 2.게시글과 댓글 작성시 익명기능

게시글이나 댓글을 작성할 때 익명으로 글/댓글을 작성할 건지, 닉네임이 나타나게 글/댓글을 작성할 건지 체크박스를 통해 입력을 받아 처리할 수 있도록 기능을 구현했다.
Activity클래스에서 레이아웃의 체크박스를 findViewid해서 가져와
if (checkBox_b.isChecked()) {
result = "익명"; // 글을 쓸 때 익명 체크박스를 체크하면 익명으로 저장
} else {
result = name; // 글을 쓸 때 익명 체크박스를 체크하지 않으면 닉네임으로 저장
}
sChecked()로 체크 여부에 따라 작성자를 익명인지, 닉네임인지를 result 변수에 저장하고, 그 변수를 다른 글/댓글 데이터와 함께 Firebase DB로 보낸다. 그 후에 글/댓글을 받아와 출력해 보여줄 때는 그냥 테이 블 내의 result를 받아와 보여주면 되도록 구현했다.
 
<img width="740px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225321-5f1fd900-c8c4-11eb-91ad-1386aa59439c.png"><br/>
(CommentActivity.java, 댓글 작성하는 코드 String result_c=“”; result_c 변수를 하나 만들고, 댓글 옆에 익명 체크박스 여부에 따라 각각 익명인지, 닉네임인지 할당하고, 그걸 CommentInfo 배열리스트에 담아 RealtimeDatabase의 Comment라는 DB 테이블에 보냄, 게시글 작성하는 WriteActivity도 동일. 1번 사진 참고)
<br/>
<br/>
  > #### 3.게시글 사진 첨부 기능

Firebase의 RealtimeDatabse나 FireStore은 이미지 파일은 담지 못 한다. 그래서 이미지 파일이나 동영상 파일을 저장하기 위해서는 storage를 활용해야만 했다.
RealtimeDatabase처럼 Firebase 사이트에서 Storage를 시작하고 프로젝트 권한을 읽고 쓸 수 있도록 권한을 수정해준다. 그 후 안드로이드 스튜디오 내에 스토리지 라이브러리를 implement로 넣어주면 스토리지에 파일을 넣고 불러올 준비는 끝난다.
먼저 이미지 파일을 넣는 작업은 게시글을 작성하는 클래스, WriteActivity에서 일어난다. storage 인스턴스를 만들고, 참조한다. 그 후 파일을 저장할 때의 파일명을 만들어주고, storage 폴더 경로에 접근해 게시글 사진을 저장할 경로 지정해준다. 그리고 그 경로에 이미지 파일을 저장해준다.
 
<img width="800px" height="170px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225373-6ba43180-c8c4-11eb-865e-bae8254e61f3.png"><br/>
<img width="800px" height="270px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225382-6e068b80-c8c4-11eb-9f39-c79c4d97d852.png"><br/>
(WriteActivity.java)<br/>
나는 여기서 각 게시글에 이미지가 첨부됐는지, 안 됐는지, 첨부됐다면 몇 번 글에 어떤 이미지가 들어갔는지 구분하기 위해 이미지 파일 명을 board_num(각 게시글 번호)+”번째 글 사진.png”로 파일명을 지정해주었다. 그리고 filePath != null이면 이미지 파일을 첨부한다는 조건으로, int image변수를 하나 만들어 첨부하면 1, 첨부하지 않으면 0으로 할당해, 게시글 DB 테이블에 글에 대한 정보로 같이 저장하였다.
그 후에 게시글을 클릭해서 게시글에 대한 정보와, 이미지 파일을 불러올 때 board_num의 child인 image가 0인지, 1인지에 따라 이미지가 첨부된 글인지 판단할 수 있도록 했다. 여기서 만약 이미지가 첨부된 게시글을 클릭해, 이미지 파일을 불러올 경우를 보겠다.
<br/>
<img width="800px" height="200px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225422-778ff380-c8c4-11eb-9e2f-1391c4100ff2.png"><br/>
(BoardImageActivity.java)<br/>
이미지가 첨부된 게시글에 대한 데이터를 가져와 보여주는 BoardImageActivity에서 보낼 때와 마찬가지처럼 먼저 storage인스턴스를 만들고, 참조한다. 그 다음 내가 찾기 위한 사진이 있는 경로를 찾아준다. 나는 storage파일/images/board_num+”번째 글 사진.png” 경로이기 때문에 storageRef.child("images/"+board_num+"번째 글 사진.png”)라고 작성했고, 이 뒤에 바로 getDownloadUrl().addOnSuccessListener(new OnSuccessListener<Uri>() 라고 Url을 받아오는 코드를 작성해준다. 이미지 파일의 url을 받아오는데 성공(onSuccess)하면
Glide.with(getApplicationContext()).load(uri).into(board_image);
Glide라이브러리를 이용해 값을 받아 ImageView에 해당 이미지 uri값을 할당해서 넣어주면 된다.
<br/><br/>
   
  > #### 4.	각 게시글 댓글 작성
  
게시판의 게시글 목록에서 클릭한 게시글의 내용을 불러와 보여주는 것까지 구현한 후에, 각 게시글의 댓글을 달 수 있는 기능을 추가하기 시작했다. 사실 게시글 구현한 방식과 거의 유사하다. 게시글을 보여주는 레이아웃에 게시글 목록을 보여주는 RecyclerView처럼 댓글 목록을 RecyclerView로 구성했다. 대신 게시글 목록을 보여주는 RecyclerView는 한 화면을 꽉 채웠다면 댓글 목록을 보여주는 화면에는 그 댓글을 단 게시글 내용도 같이 보여주어야 하기 때문에 게시글 내용 밑에 작게 recyclerview를 구성해놓았다. 또 게시글을 작성할 땐 글작성 버튼을 먼저 누르고, Intent로 글작성 레이아웃에서 작성한 후 DB로 보냈다면, 댓글을 DB를 보내고 불러오는 과정이 한 화면에서 이루어져야하기 때문에 CommentActivity(이미지 첨부 없는 게시글), BoardImageActivity(이미지 첨부 있는 게시글)내에서만 코드가 이뤄진다.
 
<img width="740px" height="120px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225455-7f4f9800-c8c4-11eb-9d43-2a81fe60d712.png"><br/>
(WriteActivity.java, board_num 저장하는 부분)<br/>
 
<img width="740px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225477-84ace280-c8c4-11eb-85c1-d3c4d9af4bf5.png"><br/>
(CommentActivity.java, 댓글 작성해 DB 보내는 부분)<br/>
 
<img width="740px" height="140px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225493-89719680-c8c4-11eb-90da-363a2d27d37f.png"><br/>
(CommentActivity.java, BoardAdapter에서 몇 번 글을 클릭했는지, 그 글의 데이터와 함께 intent로 쏴준 걸 받아온다.)<br/>
그리고 댓글을 작성하고 불러오는데에는 큰 어려움 없이 구현했지만, 특정 게시글에 대한 댓글을 골라 불러오는데 몇 일 어려움을 겪었다. 파이어베이스는 mySql처럼 DB를 추가할 때마다 수가 늘어나는 auto_increment기능이 없기 때문에 어떻게 board_num이라는 변수를 만들어 수를 계속 늘릴 수 있을까 고민했다. 그냥 DB를 보낼 때마다 +1을 하면 board_num이 계속 1로만 들어가고, 점점 늘리려면 전 글의 board_num을 불러오고 또 +1을 한 다음에 새로운 다음 글 테이블을 만들 때 넣어야 하기 때문에 코드가 쓸데없이 복잡해지고, 비효율적이기 때문에 다른 방법을 고안해냈다.
내가 게시글을 RecyclerView를 통해 각 게시글을 item레이아웃으로 나타내는데, 이 아이템 갯수를 받아올 수 있는 코드가 있었다. RecyclerView에 대한 코드들은 어댑터에서 이뤄지기 때문에
BoardAdapter.getCount(); 이렇게 작성하면 게시글 목록의 개수를 구해올 수 있었다.
즉 글을 새로 작성할 때 저 코드를 통해 작성된 게시글 개수를 받아와서 +1을 해준 다음 board_num에 넣어주고 DB로 보낸다면 각 글에 맞는 board_num을 게시글 테이블에 넣어주는 것이다.
 
<img width="740px" height="170px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225521-8f677780-c8c4-11eb-9a62-b22def1ac514.png"><br/>
(CommentActivity.java)<br/>
  
그 후에 댓글을 불러올 때 myRef_c.orderByChild("board_num").equalTo(board_num)
Firebase에서 댓글 DB를 저장한 Comment테이블을 참조(myRef_c)하고 orderByChild를 사용하여 “board_num”이 포함된 데이터를 가져온다. 그 다음 “board_num”의 값이 각 게시글에 맞는 번호 board_num인 댓글 데이터를 가져오는 방식이다.
Ex) 게시글 목록에서 1번 글을 클릭해 1번 글에 대한 내용을 보여주는 레이아웃에서 댓글을 달면 댓글 데이터는 board_num=1로 저장이 된다. 그 후에 1번 글을 보면 board_num을 가지고 있는 댓글 데이터들 중에 board_num이 1인 댓글 데이터를 불러오는 것

<img width="300px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225557-99897600-c8c4-11eb-83de-cd2e700a22d4.png"><img width="300px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225575-9ee6c080-c8c4-11eb-8458-0ff958f1bd00.png"><br/>
  (RealtimeDatabase 테이블, 저 1번글, 2번글에 있는 1,2,3.. 숫자가 board_num, 1번글 안에 있는 board_num은 각 글에 맞는 번호 저장한 것)<br/>
  <br/>

  > #### 5.	각 게시글 좋아요 기능
  
게시글 좋아요 기능도 위에 댓글 구현 방법과 유사하다. board_num을 통해 게시글을 구분해 DB를 넣고 불러오는데, 댓글 구현 방법과 다른 점은 DB를 보내는 코드와 받아오는 코드가 따로따로 작성되는 댓글과 달리, 좋아요는 DB를 불러오는 코드 안에 DB를 보내는 코드가 작성된다.
먼저 Like 테이블을 참조해 데이터를 가져오는데, 조건을 걸고 가져온다. Like테이블의 자식 board_num번글, 또 그의 자식인 like_count를 가져와서 좋아요 개수에 따라 클릭하면 늘리거나, 줄이거나하고 다시 DB에 넣어주는 코드를 작성한다. 대신 클릭할 때마다 board_num번글 좋아요에 대한 테이블을 지워준다. 지워주지 않으면 like_count가 바뀔 때마다 새로운 테이블이 생성되게 되기 때문에 꼭 지워줘야 한다.

<img width="740px" height="450px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225605-a73efb80-c8c4-11eb-826d-f2f565ebd336.png"><br/>
<img width="740px" height="450px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225627-ad34dc80-c8c4-11eb-8fdf-260e76574806.png"><br/>
(CommentActivity.java, 좋아요 버튼 이벤트 코드)<br/>
 
<img width="300px" height="400px" alt="image" src="https://user-images.githubusercontent.com/70475213/121225654-b32abd80-c8c4-11eb-8239-d1f47a9cd7a2.png"><br/>
(RealtimeDatabase의 Like(좋아요)테이블, 이 상태에서 2번 글의 좋아요를 누르면 2번 글의 like_count가 1이 되고, 1번 글의 좋아요를 취소하면 1번 글의 like_count가 0이 된다.)<br/>

  ---
  
  -시간표
  
  -채팅
  
  채팅의 주요기능은 실시간으로 대화를 할 수 있다는 것이다. 
  
  ![image](https://user-images.githubusercontent.com/80017979/121243122-4f11f480-c8d8-11eb-8e60-4117003bff26.png)
  ![image](https://user-images.githubusercontent.com/80017979/121243196-694bd280-c8d8-11eb-83a9-0bc1e3e1301b.png)
  ![image](https://user-images.githubusercontent.com/80017979/121243210-6b159600-c8d8-11eb-80c7-ef323e30b803.png)

  Firebase의 Realtime database를 이용하여 제작하였다. ChatActivity를 중심으로 ChatAdapter를 이용하여 firebase에 전송하고 받아오도록 하였다. 
  뿐만 아니라 닉네임을 설정할 수 있으며 설정한 닉네임으로 채팅을 보낸다.
  
  ![image](https://user-images.githubusercontent.com/80017979/121243227-723ca400-c8d8-11eb-996a-461099ab5a05.png)

  실시간으로 채팅을 주고받을 수 있으며 addChildEventListener를 이용하여 채팅을 주고받는 것을 구현하였다. 
  원래는 단체채팅을 가능하게 하는 것이 목표였지만 채팅을 하는 채팅방안에 2명 이상의 유저를 등록하는 것을 실패하여 결국 구현하지 못하였다
  
  -셔틀버스시간표
  
  -버스시간표
  
  -택시 모집
  
## 다른 어플과 SMUtime의 차이점

## 최종 프로젝트 링크

## 실행 영상

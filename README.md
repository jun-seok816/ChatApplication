<div><img width="2000" src ="https://user-images.githubusercontent.com/72478198/98082103-89875f00-1ebb-11eb-8965-e1abd560efd2.PNG"></div>
# ChatActivity.class

# FirebaseDatabase.getInstance()

## Description 

- Firebase DB관리 객체를 가져옴
  - https://firebase.google.com/docs/database/android/read-and-write?hl=ko#get_a_databasereference
            
## Parameter

- 없음
    
## Return
- type : void

- value : 없음

## Dependence function

- 없음

## Source code
```
firebaseDatabase= FirebaseDatabase.getInstance();
```

# firebaseDatabase.getReference()

## Description 

- chat노드의 Reference객체를 얻어옵니다.
            
## Parameter

- chat
    
## Return
- type : void

- value : 없음

## Dependence function

- 없음

## Source code

```
chatRef= firebaseDatabase.getReference("chat");
```

# addChildEventListener(new ChildEventListener(){}

## Description 

- FirebaseDB에서 채팅 메세지 실시간 일어옴
- 'chat'노드에 저장되어 있는 데이터들을 읽어오기
- chatRef에 데이터가 변경되는 것을 듣는 리스너 추가
- https://firebase.google.com/docs/reference/android/com/google/firebase/database/ChildEventListener
            
## Parameter

- 없음
    
## Return
- type : void

- value : 없음

## Dependence function

- onChildAdded()
  - MessageItem messageItem= dataSnapshot.getValue(MessageItem.class);
  - 새로 추가된 데이터(값 : MessageItem객체) 가져오기
  
  - messageItems.add(messageItem);
  - 새로운 메세지를 리스뷰에 추가하기 위해 ArrayList에 추가
  
  - adapter.notifyDataSetChanged();
  - 리스트뷰를 갱신
  
  - listView.setSelection(messageItems.size()-1);
  - 리스트뷰의 마지막 위치로 스크롤 위치 이동
  - https://firebase.google.com/docs/reference/android/com/google/firebase/database/ChildEventListener#public-abstract-void-onchildadded-datasnapshot-snapshot,-string-previouschildname


## Source code

```
chatRef.addChildEventListener(new ChildEventListener() {
            @Override
            public void onChildAdded(@NonNull DataSnapshot dataSnapshot, @Nullable String s) {

                MessageItem messageItem= dataSnapshot.getValue(MessageItem.class);

                messageItems.add(messageItem);

                adapter.notifyDataSetChanged();
                listView.setSelection(messageItems.size()-1); 
            }

            @Override
            public void onChildChanged(@NonNull DataSnapshot dataSnapshot, @Nullable String s) {

            }

            @Override
            public void onChildRemoved(@NonNull DataSnapshot dataSnapshot) {

            }

            @Override
            public void onChildMoved(@NonNull DataSnapshot dataSnapshot, @Nullable String s) {

            }

            @Override
            public void onCancelled(@NonNull DatabaseError databaseError) {

            }
        });

    }
```

# clickSend()

## Description 

- EditText메소드에 쓴 채팅을 채팅창에 띄움
            
## Parameter

- View view
  - 버튼 채팅등 안드로이드 화면을 구성하는 기본 요소
    
## Return

- type : void

- value : 없음

## Dependence function

- String time=calendar.get(Calendar.HOUR_OF_DAY)+":"+calendar.get(Calendar.MINUTE);
  - 현재 시간을 문자열로

- MessageItem messageItem= new MessageItem(nickName,message,time,pofileUrl);
  - firebase DB에 저장할 값(MessageItem객체) 설정

- chatRef.push().setValue(messageItem);
  - 'char'노드에 MessageItem객체를 통해 데이터 삽입

- InputMethodManager imm=(InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
- imm.hideSoftInputFromWindow(getCurrentFocus().getWindowToken(),0);
  - 소프트키패드 객체 생성 , 소프트키패드 올라와 있는거 숨김
  - https://blog.yena.io/studynote/2017/12/16/Android-HideKeyboard.html

## Source code
```
public void clickSend(View view) {

        String nickName= G.nickName;
        String message= et.getText().toString();
        String pofileUrl= G.porfileUrl;
        
        Calendar calendar= Calendar.getInstance();
        String time=calendar.get(Calendar.HOUR_OF_DAY)+":"+calendar.get(Calendar.MINUTE);

        MessageItem messageItem= new MessageItem(nickName,message,time,pofileUrl);
        chatRef.push().setValue(messageItem);

        et.setText("");
        InputMethodManager imm=(InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
        imm.hideSoftInputFromWindow(getCurrentFocus().getWindowToken(),0);

    }
```

# ChatAdapter.class

# ChatAdapter()

## Description 

- messageItems 와 layoutInflater변수 초기화
- https://developer.android.com/reference/android/view/LayoutInflater?hl=en
            
## Parameter

- 없음
    
## Return
- type : void

- value : 없음

## Dependence function

- 없음

## Source code

```
public ChatAdapter(ArrayList<MessageItem> messageItems, LayoutInflater layoutInflater) {
        this.messageItems = messageItems;
        this.layoutInflater = layoutInflater;
    }
```

# getCount()

## Description 

- 어댑터가 나타내는 데이터 세트에있는 항목 수입니다.
  - https://developer.android.com/reference/android/widget/HeaderViewListAdapter#getCount()
            
## Parameter

- 없음
    
## Return
- type : int

- value :데이터 항목 수

## Dependence function

- 없음

## Source code

```
public int getCount() {
        return messageItems.size();
    }
```

# getItem()

## Description 

- 데이터 세트의 지정된 위치와 연관된 데이터 항목을 가져옵니다.
  - https://developer.android.com/reference/android/widget/HeaderViewListAdapter#getItem(int)
            
## Parameter

- position
  - 데이터를 원하는 항목의 위치
    
## Return

- type : object

- value : 지정된 위치의 데이터입니다.

## Dependence function

- 없음

## Source code

```
public Object getItem(int position) {
        return messageItems.get(position);
    }
```

# getItemId()

## Description 

- 목록에서 지정된 위치와 관련된 행 ID를 가져옵니다.
  - https://developer.android.com/reference/android/widget/HeaderViewListAdapter#getItemId(int)
            
## Parameter

- position
  - 원하는 행 ID를 가진 어댑터의 데이터 세트 내 항목의 위치.
    
## Return

- type : long

- value : 지정된 위치에있는 항목의 ID입니다.

## Dependence function

- 없음

## Source code

```
 public long getItemId(int position) {
        return position;
    }
```


# getView()

## Description 

- 데이터 세트의 지정된 위치에 데이터를 표시하는View를 가져옵니다.
  - https://developer.android.com/reference/android/widget/Adapter#getView(int,%20android.view.View,%20android.view.ViewGroup)
            
## Parameter

- position
  - 데이터를 원하는 항목의 위치
- view  
  -지정된 위치의 데이터에 해당하는 View입니다.
  
- viewGroup
  - View가 최종적으로 첨부 될 상위
    
## Return

- type : View

- value : 버튼 텍스트등 안드로이드 화면을 구성하는 요소

## Dependence function

-  if(item.getName().equals(G.nickName)){
            itemView= layoutInflater.inflate(R.layout.my_msgbox,viewGroup,false);
        }else{
            itemView= layoutInflater.inflate(R.layout.other_msgbox,viewGroup,false);
        }
  - 메세지가 자신의 메세지이면 좌측에 다른 사람의 메세지면 우측에 띄움 

- Glide.with(itemView).load(item.getPofileUrl()).into(iv);
  - Glide는 원격 이미지를 가져오고 크기를 조정하고 표시해야하는 거의 모든 경우에도 효과적입니다.
  - https://github.com/bumptech/glide

## Source code

```
 @Override
    public View getView(int position, View view, ViewGroup viewGroup) {

        //현재 보여줄 번째의(position)의 데이터로 뷰를 생성
        MessageItem item=messageItems.get(position);

        //재활용할 뷰는 사용하지 않음!!
        View itemView=null;

        //메세지가 내 메세지인지??
        if(item.getName().equals(G.nickName)){
            itemView= layoutInflater.inflate(R.layout.my_msgbox,viewGroup,false);
        }else{
            itemView= layoutInflater.inflate(R.layout.other_msgbox,viewGroup,false);
        }

        //만들어진 itemView에 값들 설정
        CircleImageView iv= itemView.findViewById(R.id.iv);
        TextView tvName= itemView.findViewById(R.id.tv_name);
        TextView tvMsg= itemView.findViewById(R.id.tv_msg);
        TextView tvTime= itemView.findViewById(R.id.tv_time);

        tvName.setText(item.getName());
        tvMsg.setText(item.getMessage());
        tvTime.setText(item.getTime());

        Glide.with(itemView).load(item.getPofileUrl()).into(iv);

        return itemView;
    }
```

# MessageItem.class

# MessageItem()

## Description 

- 이름 메세지 시간 프로필 변수 초기화
            
## Parameter

- String name, String message, String time, String pofileUrl
    
## Return

- type : void

- value : 없음

## Dependence function

- 없음

## Source code

```
public MessageItem(String name, String message, String time, String pofileUrl) {
        this.name = name;
        this.message = message;
        this.time = time;
        this.pofileUrl = pofileUrl;
    }
```

# getter & setter

```
 public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getTime() {
        return time;
    }

    public void setTime(String time) {
        this.time = time;
    }

    public String getPofileUrl() {
        return pofileUrl;
    }

    public void setPofileUrl(String pofileUrl) {
        this.pofileUrl = pofileUrl;
    }
}
```

# mainActivity.class

# clickImage()

## Description 

- 프로필 이미지 선택하도록 Gallery 앱 실행
            
## Parameter

- View view
    
## Return
- type : void

- value : 없음

## Dependence function

- intent.setType()
  - 반환 할 데이터 유형을 나타 내기 위해 데이터가 아닌 유형 만 지정하는 인 텐트를 만드는 데 사용됩니다.
  - https://developer.android.com/reference/android/content/Intent#setType(java.lang.String)

## Source code

```
public void clickImage(View view) {
        //프로필 이미지 선택하도록 Gallery 앱 실행
        Intent intent= new Intent(Intent.ACTION_PICK);
        intent.setType("image/*");
        startActivityForResult(intent,10);
    }
```

# onActivityResult()

## Description 

- startActivityForResult메소드의 콜백 메소드
  - https://developer.android.com/reference/android/app/Activity?hl=ko#onActivityResult(int,%20int,%20android.content.Intent)
            
## Parameter

- requestCode: 대화상자 액티비티랑 MainActivity를 구별하기 위한 코드

- resultCode: 어떠한 결과코드를 주었는지에 대한 변수

- Intent data: 액티비티에서 보낸 결과 데이터가 들어가있는 부분
  - https://developer.android.com/reference/android/app/Activity?hl=ko#onActivityResult(int,%20int,%20android.content.Intent)
    
## Return

- type : void

- value : 없음

## Dependence function

- picasso.get().load(imgUri).into(ivProfile);
  - 피카소 객체를 얻어와 Picasso 객체의 load()를 불러 이미지를 로드하고, into()로 원하는 ImageView에 로드된 이미지를 표시하는 방식이다.
  - 이미지를 얻어오는데 퍼미션이 없어도 되는게 가장 큰 장점
  - https://calvinjmkim.tistory.com/11

## Source code

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        switch (requestCode){
            case 10:
                if(resultCode==RESULT_OK){
                    imgUri= data.getData();
                    
                    Picasso.get().load(imgUri).into(ivProfile);

                    isChanged=true;
                }
                break;
        }
    }
```
# clickBtn()

## Description 

- 채팅방 입장버튼
            
## Parameter

- View view
    
## Return

- type : void

- value : 없음

## Dependence function

- startActivity(intent)
  - ChatActivity로 전환

- savdData()
  - 새로운 프로필 data save작업

## Source code

```
public void clickBtn(View view) {

        //바꾼것도 없고, 처음 접속도 아니고..
        if(!isChanged && !isFirst){
            //ChatActivity로 전환
            Intent intent= new Intent(this, ChatActivity.class);
            startActivity(intent);
            finish();
        }else{
            //1. save작업
            saveData();
        }
    }
```

# saveData()

## Description 

- 프로필과 이름을 Firebase db 에 저장
            
## Parameter

- 없음
    
## Return
- type : void

- value : 없음

## Dependence function

- sdf.format(new Date())+".png";
  - Firebase storage에 이미지 저장하기 위해 파일명 만들기(날짜를 기반으로)

- FirebaseStorage.getInstance()
  - Firebase storage의 인스턴스를 얻어옴

- final StorageReference imgRef= firebaseStorage.getReference("profileImages/"+fileName);
  - Firebase storage에 저장
- https://firebase.google.com/docs/reference/android/com/google/firebase/storage/StorageReference

- UploadTask uploadTask=imgRef.putFile(imgUri);
  - 파일 업로드
  - https://firebase.google.com/docs/reference/android/com/google/firebase/storage/UploadTask#protected-method-summary
  
- onSuccess()
  - 이미지 업로드가 송공되었을때 firebase storage의 이미지 파일 다운로드 URL을 얻어옴
  - 길어서 아래 따로 추가설명
  
## Source code

```
void saveData(){
        G.nickName= etName.getText().toString();

        if(imgUri==null) return;

        SimpleDateFormat sdf= new SimpleDateFormat("yyyMMddhhmmss"); //20191024111224
        String fileName= sdf.format(new Date())+".png";

        FirebaseStorage firebaseStorage= FirebaseStorage.getInstance();
        final StorageReference imgRef= firebaseStorage.getReference("profileImages/"+fileName);

        UploadTask uploadTask=imgRef.putFile(imgUri);
        uploadTask.addOnSuccessListener(new OnSuccessListener<UploadTask.TaskSnapshot>() {
            @Override
            public void onSuccess(UploadTask.TaskSnapshot taskSnapshot) {
                imgRef.getDownloadUrl().addOnSuccessListener(new OnSuccessListener<Uri>() {
                    @Override
                    public void onSuccess(Uri uri) {
                        G.porfileUrl= uri.toString();
                        Toast.makeText(MainActivity.this, "프로필 저장 완료", Toast.LENGTH_SHORT).show();

                        FirebaseDatabase firebaseDatabase=FirebaseDatabase.getInstance();
                  
                        DatabaseReference profileRef= firebaseDatabase.getReference("profiles");

                        
                        profileRef.child(G.nickName).setValue(G.porfileUrl);

                        SharedPreferences preferences= getSharedPreferences("account",MODE_PRIVATE);
                        SharedPreferences.Editor editor=preferences.edit();

                        editor.putString("nickName",G.nickName);
                        editor.putString("profileUrl", G.porfileUrl);

                        editor.commit();
                       
                        Intent intent=new Intent(MainActivity.this, ChatActivity.class);
                        startActivity(intent);
                        finish();

                    }
                });
            }
        });
    }//saveData() ..
    
```
# onSuccess()

## Dependence function

- imgRef.getDownloadUrl().addOnSuccessListener()
  - firebase storage의 이미지 파일 다운로드 URL을 얻어오기
  
- G.porfileUrl= uri.toString();
  - 파라미터로 firebase의 저장소에 저장되어 있는 이미지에 대한 다운로드 주소(URL)을 문자열로 얻어오기
  
- FirebaseDatabase firebaseDatabase=FirebaseDatabase.getInstance();
  - firebase DB관리자 객체 소환
- DatabaseReference profileRef= firebaseDatabase.getReference("profiles");
  - 'profiles'라는 이름의 자식 노드 Reference 객체 얻어오기
  
- profileRef.child(G.nickName).setValue(G.porfileUrl);  
  - 닉네임을 key 식별자로 하고 프로필 이미지의 주소를 값으로 저장
  
- SharedPreferences preferences= getSharedPreferences("account",MODE_PRIVATE);
  - SharedPreferences는 데이터를 파일로 저장을 하는데, 파일이 앱 폴더 내에 저장됨
  - https://developer.android.com/reference/android/content/SharedPreferences  
  - https://re-build.tistory.com/37
  
- SharedPreferences.Editor editor=preferences.edit();
  - Editor를 이용하여, 데이터 타입에 따라 put을 해주시면 데이터가 저장됨

# loadData()

## Description 

- 내 폰에 저잗되어 있는 프로필 정보 읽어오기
            
## Parameter

- 없음
    
## Return
- type : void

- value : 없음

## Dependence function

- getSharedPreferences()
  - 기존 폰에 저장된 계정정보를 불러옴
  - https://developer.android.com/reference/android/content/Context#getSharedPreferences(java.lang.String,%20int)
  
- preferences.getString()
  - 저장된 계정정보에서 nickname 이랑 profile을 전역변수로 초기화
  - https://developer.android.com/reference/android/content/Context#getString(int,%20java.lang.Object...)  

## Source code

```
 void loadData(){
        SharedPreferences preferences=getSharedPreferences("account",MODE_PRIVATE);
        G.nickName=preferences.getString("nickName", null);
        G.porfileUrl=preferences.getString("profileUrl", null);


    }

```


# G.class

## Description 

- 내 phone에 저장(내폰에 저장된 값으로 로그인을 계속 안하기 위해) 위해선 그 데이터 값을 계속 갖고 있을 전역 변수가 필요하므로..G라는 class를 만들어서 매개 변수로 값을 저장.

## source code

```
public class G {

    public static String nickName;
    public static String porfileUrl;
}

```

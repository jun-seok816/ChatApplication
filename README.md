# ChatActivity 메소드

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
- 소프트키페드 객체 생성 , 소프트키포드 올라와 있는거 숨김
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

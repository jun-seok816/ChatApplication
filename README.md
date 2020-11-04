# ChatActivity 메소드

# FirebaseDatabase.getInstance()

## Description 

- Firebase DB관리 객체를 가져옴
  - https://firebase.google.com/docs/database/android/read-and-write?hl=ko
            
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

# initializeWebView메소드

## Description 

- 자바스크립트에서 안드로이드 메소드를 인식할 수 있게 설정하는 메소드.
- 제공된 이름을 사용하여 자바스크립트에서 안드로이드 메소드에 엑세스 할 수 있습니다.
            
## Parameter

- 없음
    
## Return
- type : void

- value : 없음

## Dependence function

## Source code

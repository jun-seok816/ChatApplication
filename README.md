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

# getView()

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


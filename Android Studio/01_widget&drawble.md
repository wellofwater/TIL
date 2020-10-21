# 위젯

### 기본 위젯

- 텍스트뷰; **TextView**
  - `text` : 화면의 문자열을 설정
- 에디트텍스트; **EditText**
  - 사용자에게 값을 입력받을 때 사용
  - `inpuType` : 한글, 영문, 숫자 등 입력받는 문자의 유형에 따라 설정
- 버튼; **Button**
  - 클릭 이벤트 발생, **OnClickListener**: `void onClick(View v)`
  - 기본 버튼, 체크 박스, 라디오 버튼 등
- 이미지뷰, 이미지 버튼; **ImageView**, **ImageButton**
  - 이미지를 화면에 표시
  - `app:srcCompat="@drawable/이미지 파일명"` : 이미지 리소스 지정 방식으로 이미지 지정



#### 토스트; Toast

> 간단한 메시지를 잠깐 보여주었다가 없어지는 뷰

```java
//Toast.makeText(Context context, String message, int duration).show();
Toast.makeText(this, "토스트 출력", Toast.LENGTH_SHORT).show();
```

- 사용자 정의

```java
public void clickb2(View v){
    // 토스트를 위한 레이아웃 인플레이션
    LayoutInflater inflater = getLayoutInflater();
    View view = inflater.inflate(R.layout.toast, (ViewGroup) findViewById(R.id.toast_layout));
    
    TextView tv = view.findViewById(R.id.textView);
    tv.setText("INPUT TEXT");

    // 토스트 생성
    Toast t = new Toast(this);
    // 위치 지정
    t.setGravity(Gravity.CENTER, 0, 0);
    // 디스플레이 시간 설정
    t.setDuration(Toast.LENGTH_LONG);
    // 토스트가 보이는 뷰 설정
    t.setView(view);
    t.show();
    }
```





#### 스낵바; Snackbar

> 간단한 메시지 출력, but 외부 라이브러리로 추가
>
> 머티리얼 라이브러리(Material Library)에 정의

```java
//Snackbar.makeText(View view, String message, int duration).show();
Snackbar.makeText(v, "스낵바 출력", Snackbar.LENGTH_long).show();
```



#### 알림 대화상자; AlertDialog

```java
public void clickb4(View v){
    // 대화상자를 만들기 위한 빌더 객체 생성
    AlertDialog.Builder builder = new AlertDialog.Builder(this);
    
    // 대화상자 셋팅
    builder.setTitle("My Dialog");
    builder.setMessage("Are you Exit Now");
    builder.setIcon(R.drawable.maple);

    // 예 버튼 추가
    builder.setPositiveButton("Yes", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            finish();
        }
    });

    // 아니오 버튼 추가
    builder.setNegativeButton("No", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            // 이벤트 입력
        }
    });
    
    // 취소 버튼 추가
    builder.setNeutralButton("취소", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            
        }
    });

    // 대화상자 객체 생성 후 보여주기
    AlertDialog dialog = builder.create();
    dialog.show();
}
```



#### 프로그레스바

> 작업의 진행 정도나 작업이 진행 중임을 표시

- 막대 모양
- 원 모양



# 드로어블

- 비트맵 드로어블; **BitmapDrawble**
  - 비트맵 그래픽 파일(png, jpg, gif 등)을 사용해서 생성
- 상태 드로어블; BitmapDrawble
  - 상태별로 다른 비트맵 그래픽을 참조
- 전환 드로어블; TransitionDrawble
  - 두 개의 드로어블을 전환
- 셰이프 드로어블; ShapeDrawble
  - 도형 모양을 정의
- 인셋 드로어블; InsetDrawble
- 클립 드로어블; ClipDrawble
- 스케일 드로어블; ScaleDrawble



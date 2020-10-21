# 인플레이션; inflation

> 화면의 일부분을 XML 레이아웃 파일 내용으로 적용
>
> `LayoutInflater` 객체의 `inflate(int resource, ViewGroup root)` 메서드 사용
>
> - inflate(불러올 XML 레이아웃 리소스, 부모 컨테이너) 지정



##### MainActivity.java

```java

public class MainActivity extends AppCompatActivity {
    LinearLayout container;
    String strId, strPwd, strName, strEmail;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        container = findViewById(R.id.container);

        strId = "id01";
        strPwd = "1234";
        strName = "김말자";
        strEmail = "email@gmail.com";
    }

    public void clickbtn(View v){
        ProgressDialog progressDialog = null;

        if(v.getId() == R.id.button){
            // 프로그래스 바 생성
            progressDialog = new ProgressDialog(this);
            progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
            progressDialog.setTitle("login");
            progressDialog.show();
            
            // 이전 화면 지우기
            container.removeAllViews();
            
            // LayoutInflater 객체 참조
            LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            // 전환할 레이아웃 설정
            inflater.inflate(R.layout.login, container, true);

            // 회원정보를 가져와 TextView에 출력
            TextView textViewId = container.findViewById(R.id.textViewID);
            TextView textViewPwd = container.findViewById(R.id.textViewPwd);
            TextView textViewName = container.findViewById(R.id.textViewName);
            TextView textViewEmail = container.findViewById(R.id.textViewEmail);
            textViewId.setText(strId);
            textViewPwd.setText(strPwd);
            textViewName.setText(strName);
            textViewEmail.setText(strEmail);
        }else if(v.getId() == R.id.button2){
            // 대화상자(다이얼로그) 생성
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.setTitle("Sign up");
            builder.setMessage("가입하시겠습니까?");
            builder.setIcon(R.drawable.maple);

            builder.setPositiveButton("Yes", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialogInterface, int i) {
                    container.removeAllViews();
                    LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                    inflater.inflate(R.layout.signup, container, true);
                }
            });

            builder.setNegativeButton("No", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialogInterface, int i) {
                }
            });

            AlertDialog dialog = builder.create();
            dialog.show();
            // 화면 전환 없음
        }else if (v.getId() == R.id.button3){
            // 이전 화면 지우기
            container.removeAllViews();
            
            // LayoutInflater 객체 참조
            LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            // 전환할 레이아웃 설정
            inflater.inflate(R.layout.userinfo, container, true);
            
            // 기존 회원정보를 EditText에 입력
            EditText updateId = container.findViewById(R.id.updateId);
            EditText updatePwd = container.findViewById(R.id.updatePwd);
            EditText updateName = container.findViewById(R.id.updateName);
            EditText updateEmail = container.findViewById(R.id.updateEmail);
            updateId.setText(strId);
            updatePwd.setText(strPwd);
            updateName.setText(strName);
            updateEmail.setText(strEmail);
        }else if (v.getId() == R.id.button4){
            // EditText의 입력 정보를 저장
            EditText updateId = container.findViewById(R.id.updateId);
            EditText updatePwd = container.findViewById(R.id.updatePwd);
            EditText updateName = container.findViewById(R.id.updateName);
            EditText updateEmail = container.findViewById(R.id.updateEmail);
            strId = updateId.getText().toString();
            strPwd = updatePwd.getText().toString();
            strName = updateName.getText().toString();
            strEmail = updateEmail.getText().toString();
            Toast.makeText(this, "Update Complete!", Toast.LENGTH_SHORT).show();
        }
    }
```


# 10/15

> 버튼을 통한 화면 전환
>
> 로그인 : 프로그래스바
>
> 회원가입 : 다이얼로그
>
> 회원정보 : 정보 수정



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
            progressDialog = new ProgressDialog(this);
            progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
            progressDialog.setTitle("login");
            progressDialog.show();


            container.removeAllViews();
            LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            inflater.inflate(R.layout.login, container, true);


            TextView textViewId = container.findViewById(R.id.textViewID);
            TextView textViewPwd = container.findViewById(R.id.textViewPwd);
            TextView textViewName = container.findViewById(R.id.textViewName);
            TextView textViewEmail = container.findViewById(R.id.textViewEmail);
            textViewId.setText(strId);
            textViewPwd.setText(strPwd);
            textViewName.setText(strName);
            textViewEmail.setText(strEmail);
        }else if(v.getId() == R.id.button2){
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
        }else if (v.getId() == R.id.button3){
            container.removeAllViews();
            LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            inflater.inflate(R.layout.userinfo, container, true);
            EditText updateId = container.findViewById(R.id.updateId);
            EditText updatePwd = container.findViewById(R.id.updatePwd);
            EditText updateName = container.findViewById(R.id.updateName);
            EditText updateEmail = container.findViewById(R.id.updateEmail);
            updateId.setText(strId);
            updatePwd.setText(strPwd);
            updateName.setText(strName);
            updateEmail.setText(strEmail);
        }else if (v.getId() == R.id.button4){
            EditText updateId = container.findViewById(R.id.updateId);
            EditText updatePwd = container.findViewById(R.id.updatePwd);
            EditText updateName = container.findViewById(R.id.updateName);
            EditText updateEmail = container.findViewById(R.id.updateEmail);
            strId = updateId.getText().toString();
            strPwd = updatePwd.getText().toString();
            strName = updateName.getText().toString();
            strEmail = updateEmail.getText().toString();
            Toast.makeText(this, "Update Complete!", Toast.LENGTH_SHORT).show();
        }else if(v.getId() == R.id.button6){
        }
    }
```


# 네트워킹; Networking

> 인터넷에 연결되어있는 원격지의 서버 또는 단말과 통신하여 데이터를 주고받는 동작
>
> 권한 필요

##### AndroidManifest

```java
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```



##### MainActivity.java

```java

public class MainActivity extends AppCompatActivity {

    ActionBar actionBar;
    BroadcastReceiver broadcastReceiver;
    IntentFilter intentFilter;

    ListView listView;
    EditText edit_name, edit_birth, edit_phone;
    TextView tx_number;
    ArrayList<Person> people;
    ConstraintLayout container;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // ActionBar 설정
        actionBar = getSupportActionBar();
        actionBar.setLogo(R.drawable.wifi);

        // 네트워크 상태 접근 권한 확인
        String[] permissions = {Manifest.permission.ACCESS_NETWORK_STATE};
        ActivityCompat.requestPermissions(this, permissions, 101);
        intentFilter = new IntentFilter();
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");

        // 네트워크 상태 데이터로 수신
        broadcastReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                String cmd = intent.getAction();
                ConnectivityManager cm = null;
                NetworkInfo mobile = null;
                NetworkInfo wifi = null;

                if(cmd.equals("android.net.conn.CONNECTIVITY_CHANGE")){
                    // 네트워크 상태에 따라 로고 설정
                    cm = (ConnectivityManager) context.getSystemService(context.CONNECTIVITY_SERVICE);
                    mobile = cm.getNetworkInfo(ConnectivityManager.TYPE_MOBILE);
                    wifi = cm.getNetworkInfo(ConnectivityManager.TYPE_WIFI);
                    if(mobile != null && mobile.isConnected()){

                    }else if (wifi != null && wifi.isConnected()){
                        actionBar.setLogo(R.drawable.wifi);
                    }else {
                        actionBar.setLogo(R.drawable.wifi2);
                    }
                }
            }
        };

        // ActionBar 설정
        actionBar.setDisplayOptions(ActionBar.DISPLAY_SHOW_HOME | ActionBar.DISPLAY_USE_LOGO);

        listView = findViewById(R.id.listView);
        container = findViewById(R.id.container);
        edit_name = findViewById(R.id.edit_name);
        edit_birth = findViewById(R.id.edit_birth);
        edit_phone = findViewById(R.id.edit_phone);
        tx_number = findViewById(R.id.tx_number);
        people = new ArrayList<>();

        tx_number.setText("0명");
    }

    class PersonAdapter extends BaseAdapter{
        ArrayList<Person> datas;

        public PersonAdapter(ArrayList<Person> datas){
            this.datas = datas;
        }

        @Override
        public int getCount() {
            return datas.size();
        }

        @Override
        public Object getItem(int i) {
            return datas.get(i);
        }

        @Override
        public long getItemId(int i) {
            return i;
        }

        @Override
        public View getView(int i, View view, ViewGroup viewGroup) {
            View v = null;
            // 사용자 정의 layout 사용
            LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            v = inflater.inflate(R.layout.person, container, true);

            TextView tx_name = v.findViewById(R.id.tx_name);
            TextView tx_birth = v.findViewById(R.id.tx_birth);
            TextView tx_phone = v.findViewById(R.id.tx_phone);

            Person p = datas.get(i);
            tx_name.setText(p.getName());
            tx_birth.setText(p.getBirth());
            tx_phone.setText(p.getPhone());

            return v;
        }
    }

    public void setList(final ArrayList<Person> people){
        // ListView에 Adapter 셋팅
        final PersonAdapter personAdapter = new PersonAdapter(people);
        listView.setAdapter(personAdapter);

        // 리스트의 Item 클릭시 이벤트 발생
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> adapterView, View view, final int position, long l) {
                // 대화상자 생성
                AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
                builder.setTitle("Delete");
                builder.setMessage(people.get(position).getName() + " 삭제하시겠습니까?");

                builder.setPositiveButton("YES", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        people.remove(position);
                        tx_number.setText(people.size() + "명");
                        personAdapter.notifyDataSetChanged();
                    }
                });
                builder.setNegativeButton("NO", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {

                    }
                });

                builder.show();
            }
        });
    }

    public void getData(){
        // 데이터 가져오기
        String name = edit_name.getText().toString();
        String birth = edit_birth.getText().toString();
        String phone = edit_phone.getText().toString();
        people.add(new Person(name,birth,phone));
        setList(people);

        tx_number.setText(people.size() + "명");
    }

    public void ckbt(View v){
        getData();
    }
}
```




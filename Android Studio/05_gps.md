# 위치기반 서비스

> 위치 관리자(LocationManager) 시스템 서비스가 관리
>
> `android.location` 패키지: 위치 정보를 확인 및 확인된 위치 정보 사용 등의 클래스가 정의



### 위치 정보 요청

1. 위치 관리자 객체 참조; getSystemService() 메서드 이용
2. 위치 리스너 구현; LocationListener
3. 위치 정보 업데이트 요청; requestLocationUpdates() 메서드 호출
4. 권한 추가; ACCESS_FINE_LOCATION



- AndroidManifest.xml

  ```java
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
  ```

- Activity.java

  ```java
  public class MainActivity extends AppCompatActivity {
      TextView textView;
      LocationManager locationManager;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          textView = findViewById(R.id.textView);
  
          // 권한 확인
          String[] permission = {
                  Manifest.permission.ACCESS_FINE_LOCATION
          };
          ActivityCompat.requestPermissions(this, permission, 101);
          // 허용하지 않을 경우 앱 종료
          if (checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION) == PackageManager.PERMISSION_DENIED) {
              Toast.makeText(this, "Finish", Toast.LENGTH_SHORT).show();
              finish();
          }
  
          // 위치 리스너 호출
          MyLocation myLocation = new MyLocation();
          // 위치 관리자 객체 참조
          locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);
          // 위치 정보 업데이트
          locationManager.requestLocationUpdates(
                  LocationManager.GPS_PROVIDER,
                  1, 0,
                  myLocation
          );
      }
  
      // 버튼 클릭 이벤트
      public void ckbt(View v) {
          StartMrLocation();
      }
  
      // 마지막 위치 정보 호출 
      private void StartMrLocation() {
          Location location = null;
          // 권한 확인
          if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
              Toast.makeText(this, "Finish", Toast.LENGTH_SHORT).show();
              finish();
          }
          
          // GPS 마지막 위치 정보
          location = locationManager.getLastKnownLocation(LocationManager.GPS_PROVIDER);
          if(location != null) {
              double lat = location.getLatitude();
              double lon = location.getLongitude();
              textView.setText(lat + "\n" + lon);
          }
      }
  
      // 위치 리스너 구현
      class MyLocation implements LocationListener{
  
          @Override
          public void onLocationChanged(@NonNull Location location) {
              double lat = location.getLatitude();
              double lon = location.getLongitude();
              textView.setText(lat + "\n" + lon);
          }
      }
  }
  ```





### 현재 위치의 지도

#### 01. 구글맵 API Key 발급

- [Google Cloud Platform Console](https://cloud.google.com/console/google/maps-apis/overview)에서 `project` 및 `사용자 인증 정보 > API 키` 생성

- Android 앱 사용량 제한 -> 패키지 이름과 SHA-1 서명 인증서 디지털 지문 추

  1. google_maps_key.xml에서 확인 가능

  2. 디버그 인증서 디지털 지문: 명령 프롬프트

     ```
     C:\Users\i>cd C:\Program Files\Java\jdk1.8.0_251\bin
     
     C:\Program Files\Java\jdk1.8.0_251\bin>keytool -list -v -keystore "%USERPROFILE%\.android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
     ```

     

#### 02. API key 삽입

- Google Play Service 라이브러리 사용 설정

  - build.gradle(Module:app)

    ```java
    dependencies {
        ...
        implementation 'com.google.android.gms:play-services-maps:17.0.0'
    }
    ```

- 매니페스트에 정보 등록

  - AndroidManifest.xml

    ```java
    // value에 API Key를 직접 사용하거나 string을 생성하여 사용
    <meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="@string/google_maps_key" />
    ```

- XML 레이아웃에 맵프래그먼트 추가

  ```java
  <fragment
      android:id="@+id/map"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:name="com.google.android.gms.maps.SupportMapFragment"/>
  ```

  

## 위치에 따른 지도 이동

### 01. 마커 표시

```java
public class MainActivity extends AppCompatActivity {
    SupportMapFragment supportMapFragment;
    GoogleMap gmap;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // 프래그먼트 객체 참조
        supportMapFragment = (SupportMapFragment) getSupportFragmentManager().findFragmentById(R.id.map);
        // getMapAsync() 메서드 호출
        supportMapFragment.getMapAsync(new OnMapReadyCallback() {
            @Override
            public void onMapReady(GoogleMap googleMap) {
                gmap = googleMap;
                
                // 위경도 좌표값
                LatLng latLng = new LatLng(37.4488, 126.4513);
                // 마커 설정
                gmap.addMarker(new MarkerOptions().position(latLng).title("인천국제공항").snippet("Incheon International Airport").icon(BitmapDescriptorFactory.fromResource(R.drawable.airport)));
                // 카메라 배율 설정
                gmap.animateCamera(CameraUpdateFactory.newLatLngZoom(latLng, 10));
            }
        });
    }

    public void ckbt1(View v){
            LatLng latLng = new LatLng(37.5131, 127.1035);
            gmap.addMarker(new MarkerOptions().position(latLng).title("롯데월드타워").snippet("Lotte world tower").icon(BitmapDescriptorFactory.fromResource(R.drawable.lottetower)));
            gmap.animateCamera(CameraUpdateFactory.newLatLngZoom(latLng, 15));
    }
}
```



### 02. 현재 위치

```java
public class MainActivity extends AppCompatActivity {
    SupportMapFragment supportMapFragment;
    GoogleMap gmap;

    TextView textView;
    LocationManager locationManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getSupportActionBar().hide();

        // 권한 확인
        String[] permission = {
                Manifest.permission.ACCESS_FINE_LOCATION,
                Manifest.permission.ACCESS_COARSE_LOCATION
        };
        ActivityCompat.requestPermissions(this, permission, 101);

        // Google Map
        supportMapFragment = (SupportMapFragment) getSupportFragmentManager().findFragmentById(R.id.map);
        supportMapFragment.getMapAsync(new OnMapReadyCallback() {
            @Override
            public void onMapReady(GoogleMap googleMap) {
                gmap = googleMap;
                if (checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION) == PackageManager.PERMISSION_DENIED
                        || checkSelfPermission(Manifest.permission.ACCESS_COARSE_LOCATION) == PackageManager.PERMISSION_DENIED) {
                    return;
                }
                
                // 현재 위치를 전송
                gmap.setMyLocationEnabled(true);
                LatLng latLng = new LatLng(37.4488, 126.4513);
                gmap.animateCamera(CameraUpdateFactory.newLatLngZoom(latLng, 10));
            }
        });

        // Location
        textView = findViewById(R.id.textView);

        MyLocation myLocation = new MyLocation();

        locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);
        locationManager.requestLocationUpdates(
                LocationManager.GPS_PROVIDER,
                1, 0,
                myLocation
        );
    } // end onCreate

    // 액티비티 비활성화: 현재 위치 전송 중지
    @SuppressLint("MissingPermission")
    @Override
    protected void onPause() {
        super.onPause();
        if(gmap != null) {
            gmap.setMyLocationEnabled(false);
        }
    }

    // 액티비티 활성화: 현재 위치 전송
    @SuppressLint("MissingPermission")
    @Override
    protected void onResume() {
        super.onResume();
        if(gmap != null){
            gmap.setMyLocationEnabled(true);
        }
    }

    class MyLocation implements LocationListener {

        @Override
        public void onLocationChanged(@NonNull Location location) {
            double lat = location.getLatitude();
            double lon = location.getLongitude();
            textView.setText(lat + "\n" + lon);

            LatLng latLng = new LatLng(lat, lon);
            gmap.animateCamera(CameraUpdateFactory.newLatLngZoom(latLng, 10));
        }
    }
}
```


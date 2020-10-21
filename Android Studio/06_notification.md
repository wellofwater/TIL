# 알림

## 01. 진동

> `Vibrator` 시스템 서비스 객체 사용

### vibrate()

> 진동이 울리는 패턴이나 시간 지정

- VIBRATE() 권한 필요 : AndroidManifest.xml

  ```java
  <uses-permission android:name="android.permission.VIBRATE" />
  ```

- 사용시 단말 버전 체크 필요

  ```java
  // long 자료형 값: 진동 지속 시간
  public void vibrate (long milliseconds)
  // android version 26부터 VibrationEffect 자료형으로 변경
  public void vibrate (VibrationEffect vibe)
  ```



**실행코드**

```java
public void ckbt1(View v){
    // Vibrator 객체 참조
    Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE);
    
    // OS 버전에 따라 다른 파라미터 형태로 설정
    if (Build.VERSION.SDK_INT >= 26) {
        // VibrationEffect.createOneShot(지속시간, 음량)
        vibrator.vibrate(VibrationEffect.createOneShot(1000, 10));
    }else {
        vibrator.vibrate(1000);
    }
}
```



## 02. 소리

### Ringtone

>  API에서 제공하는 소리를 재생
>
> `play()` 메서드를 호출하여 재생



**실행코드**

```java
public void ckbt2(View v){
    // Uri 객체를 통해 음원 지정
    // TYPE_NOTIFICATION 상수로 음원 지정
    Uri uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
    // Ringtione 객체 참조
    Ringtone ringtone = RingtoneManager.getRingtone(getApplicationContext(), uri);
    ringtone.play();
}
```



### MediaPlayer

> 직접 지정한 음원을 재생
>
> `start()` 메서드를 호출하여 재생



**실행코드**

```java
public void ckbt3(View v){
    // 지정한 음원을 불러와 객체 참조
    MediaPlayer player = MediaPlayer.create(getApplicationContext(), R.raw.mp);
    player.start();
}
```



## 03. 상단 알림

> `NotificationManager` 시스템 서비스 이용
>
> `Notification` 객체 생성시 `NotificationCompat.Builder` 객체 이용
>
> - 오레오 이후 버전은 Builder 객체 생성시 알림 채널이 필요하여, 버전 체크 필요



**실행코드**

```java
NotificationManager manager;

public void ckbt4(View v){
    // NotificationManager 객체 참조
    manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    NotificationCompat.Builder builder = null;
    
    // 단말기 OS 버전 확인
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        // 오레오 이후 버전
        if(manager.getNotificationChannel("ch1") == null){
            // 알림 채널 생성
            manager.createNotificationChannel(new NotificationChannel("ch1", "chname", NotificationManager.IMPORTANCE_DEFAULT));
        }
        // 알림 채널 지정
        builder = new NotificationCompat.Builder(this, "ch1");
    }else {
        // 오레오 전 버전
        builder = new NotificationCompat.Builder(this);
    }
    
    // PendingIntent 객체 생성
    Intent intent = new Intent(this, MainActivity.class);
    PendingIntent pendingIntent = PendingIntent.getActivity(this, 101, intent, PendingIntent.FLAG_UPDATE_CURRENT);
    
    // 알림 제목, 메시지 등 설정
    builder.setContentTitle("Noti Test");
    builder.setContentText("Content Text");
    builder.setSmallIcon(R.drawable.wifi2);
    
    // 알림을 클릭했을 때 알림 표시 삭제(true)
    builder.setAutoCancel(true);
    // 빌더에 PendingIntent 객체 설정
    builder.setContentIntent(pendingIntent);
    
    // build() 메서드를 호출하여 Notification 객체 생성
    Notification noti = builder.build();
    
    // notify() 메서드를 호출하여 알림 띄우기
    manager.notify(1, noti);
}
```





## 04. 푸시 서비스

> 단말의 상단 상태바(Status Bar)에 업데이트 메시지 표시



### 메시지 전송 방식

- 단순 SMS 를 이용한 알림

- 앱에서 서버에 연결을 만들어 놓은 상태에서 알림

- 구글의 푸시 서비스`FCM; Firebase Cloud Messaging`을 사용하여 알림
  1. 클라우드 서버에 **단말을 등록**하여 서버로부터 등록 ID 발급
  2. 애플리케이션 서버로 **등록 ID를 전송**
  3. 애플리케이션 서버에서 클라우드에 **메시지 전송**
  4. 클라우드 서버에서 대상 단말에 **메시지 수신**



### FCM 방식 앱

1. 안드로이드 프로젝트 생성

2. Firebase에 새로운 프로젝트를 생성하여 앱 추가

   - 패키지 이름 등록
   - google-service.json 파일을 다운받아 안드로이드 프로젝트의 app폴더에 저장

3. 앱에서 Firebase SDK 추가

   - build.gradle(Project: 프로젝트명)

     ```java
     dependencies {
         // 추가
         classpath 'com.google.gms:google-services:4.3.4'
     }
     ```

   - build.gradle(Module: app)

     ```java
     // 추가
     apply plugin: 'com.google.gms.google-services'
     
     dependencies {
         // 추가
         implementation 'com.google.firebase:firebase-messaging:20.0.0'
         implementation 'com.google.firebase:firebase-core:17.2.0'
     }
     ```

4. Service 파일 생성

   ```java
   public class MyFService extends FirebaseMessagingService {
       public MyFService() {
       }
   
       @Override
       public void onMessageReceived(@NonNull RemoteMessage remoteMessage) {
           // 새로운 메시지를 받았을 때 호출되는 메서드
           String title = remoteMessage.getNotification().getTitle();
           String control = remoteMessage.getData().get("control");
           String data = remoteMessage.getData().get("data");
           Log.d("[TAG]:", title + " " + control + " " + data);
   
           // Activity에 데이터를 보내기 위한 인텐트 생성
           Intent intent = new Intent("notification");
           intent.putExtra("title", title);
           intent.putExtra("control", control);
           intent.putExtra("data", data);
           // 인텐트 전송
           LocalBroadcastManager.getInstance(this).sendBroadcast(intent);
       }
   }
   ```

5. 단말 등록 기능, 메시지 수신 기능 추가; Activity.java

   ```java
   public class MainActivity extends AppCompatActivity {
       TextView tx;
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);
           tx = findViewById(R.id.tx);
   
           // 메시지 수신 확인 리스너
           FirebaseMessaging.getInstance().subscribeToTopic("car").addOnCompleteListener(new OnCompleteListener<Void>() {
               @Override
               public void onComplete(@NonNull Task<Void> task) {
                   String msg = "FCM Complete ...";
                   if(!task.isSuccessful()){
                       msg = "FCM Fail";
                   }
                   Log.d("[TAG]:", msg);
               }
           });
           LocalBroadcastManager lbm = LocalBroadcastManager.getInstance(this);
           lbm.registerReceiver(receiver, new IntentFilter("notification"));
       } // end onCreate
   
       // 수신 메시지 출력
       public BroadcastReceiver receiver = new BroadcastReceiver() {
           @Override
           public void onReceive(Context context, Intent intent) {
               if(intent != null){
                   String title = intent.getStringExtra("title");
                   String control = intent.getStringExtra("control");
                   String data = intent.getStringExtra("data");
                   tx.setText(control + " " + data);
                   Toast.makeText(MainActivity.this, title + " " + control + " " + data, Toast.LENGTH_SHORT).show();
               }
           }
       };
   }
   ```

6. 웹 서버 구현; Eclipse Tomcat - WebProject에 생성

   ```java
   @WebServlet({ "/Ftest", "/ftest" })
   public class Ftest extends HttpServlet {
   	private static final long serialVersionUID = 1L;
   
       public Ftest() {
           super();
       }
   
   
   	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   			URL url = null;
   			try {
   				url = new URL("https://fcm.googleapis.com/fcm/send");
   			} catch (MalformedURLException e) {
   				System.out.println("Error while creating Firebase URL | MalformedURLException");
   				e.printStackTrace();
   			}
   			HttpURLConnection conn = null;
   			try {
   				conn = (HttpURLConnection) url.openConnection();
   			} catch (IOException e) {
   				System.out.println("Error while createing connection with Firebase URL | IOException");
   				e.printStackTrace();
   			}
   			conn.setUseCaches(false);
   			conn.setDoInput(true);
   			conn.setDoOutput(true);
   			conn.setRequestProperty("Content-Type", "application/json");
   
   			// set my firebase server key
   			conn.setRequestProperty("Authorization", "key=" + "클라우드 메시징 서버 키");
   
   			// create notification message into JSON format
   			JSONObject message = new JSONObject();
   			message.put("to", "/topics/car");
   			message.put("priority", "high");
   			
   			JSONObject notification = new JSONObject();
   			notification.put("title", "title1");
   			notification.put("body", "body1");
   			message.put("notification", notification);
   			
   			JSONObject data = new JSONObject();
   			data.put("control", "control1");
   			data.put("data", 100);
   			message.put("data", data);
   
   			try {
   				OutputStreamWriter out = new OutputStreamWriter(conn.getOutputStream(), "UTF-8");
   				out.write(message.toString());
   				out.flush();
   				conn.getInputStream();
   				System.out.println("OK...............");
   
   			} catch (IOException e) {
   				System.out.println("Error while writing outputstream to firebase sending to ManageApp | IOException");
   				e.printStackTrace();
   			}	
   				
   	}
   
   }
   ```

   


# 10/19

> 로그인 후, 다음 페이지로 이동
>
> 영화 정보를 `kobisopenapi` 사용하여 리스트로 출력
>
> 리스트의 정보를 클릭하면 상세 내용을 AlertDialog로 출력



## eclipse

> login.jsp
>
> id, pwd는 임의설정

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%

   String id = request.getParameter("id");
   String pwd = request.getParameter("pwd");
   System.out.println(id+" "+pwd);
   Thread.sleep(3000);
   if(id.equals("id01") && pwd.equals("pwd01")){
      out.print("1");
   }else{
      out.print("2");
   }

%>
```



## Android Studio

- HttpConnect

  ```java
  package com.example.p533;
  
  import java.io.BufferedInputStream;
  import java.io.BufferedReader;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.net.HttpURLConnection;
  import java.net.URL;
  
  public class HttpConnect {
  
      public static String getString(String urlstr){
          String result = null;
          HttpURLConnection hcon = null;
          InputStream is = null;
          try{
              URL url = new URL(urlstr);
              hcon = (HttpURLConnection)url.openConnection();
              hcon.setConnectTimeout(2000);
              hcon.setRequestMethod("GET");
              is = new BufferedInputStream(hcon.getInputStream());
              result = convertStr(is);
          }catch(Exception e){
              e.printStackTrace();
          }
          return result;
      }
  
      public static String convertStr(InputStream is){
          String result = null;
          BufferedReader bi = null;
          StringBuilder sb = new StringBuilder();
          try{
              bi = new BufferedReader(new InputStreamReader(is));
              String temp = "";
              while((temp =bi.readLine()) != null){
                  sb.append(temp);
              }
          }catch(Exception e){
              e.printStackTrace();
          }
          return sb.toString();
      }
  }
  ```

  

- Movie.class

  ```java
  package com.example.p533;
  
  public class Movie {
      String rnum;
      String rank;
      String rankInten;
      String rankOldAndNew;
      String movieCd;
      String movieNm;
      String openDt;
      String salesAmt;
      String salesShare;
      String salesInten;
      String salesChange;
      String salesAcc;
      String audiCnt;
      String audiInten;
      String audiChange;
      String audiAcc;
      String scrnCnt;
      String showCnt;
  
      public Movie() {
      }
  
      public Movie(String movieNm, String audiCnt) {
          this.movieNm = movieNm;
          this.audiCnt = audiCnt;
      }
  
      public Movie(String rank, String movieNm, String openDt, String audiCnt, String scrnCnt) {
          this.rank = rank;
          this.movieNm = movieNm;
          this.openDt = openDt;
          this.audiCnt = audiCnt;
          this.scrnCnt = scrnCnt;
      }
  
      public Movie(String rnum, String rank, String rankInten, String rankOldAndNew, String movieCd, String movieNm, String openDt, String salesAmt, String salesShare, String salesInten, String salesChange, String salesAcc, String audiCnt, String audiInten, String audiChange, String audiAcc, String scrnCnt, String showCnt) {
          this.rnum = rnum;
          this.rank = rank;
          this.rankInten = rankInten;
          this.rankOldAndNew = rankOldAndNew;
          this.movieCd = movieCd;
          this.movieNm = movieNm;
          this.openDt = openDt;
          this.salesAmt = salesAmt;
          this.salesShare = salesShare;
          this.salesInten = salesInten;
          this.salesChange = salesChange;
          this.salesAcc = salesAcc;
          this.audiCnt = audiCnt;
          this.audiInten = audiInten;
          this.audiChange = audiChange;
          this.audiAcc = audiAcc;
          this.scrnCnt = scrnCnt;
          this.showCnt = showCnt;
      }
  
      public String getRnum() {
          return rnum;
      }
  
      public void setRnum(String rnum) {
          this.rnum = rnum;
      }
  
      public String getRank() {
          return rank;
      }
  
      public void setRank(String rank) {
          this.rank = rank;
      }
  
      public String getRankInten() {
          return rankInten;
      }
  
      public void setRankInten(String rankInten) {
          this.rankInten = rankInten;
      }
  
      public String getRankOldAndNew() {
          return rankOldAndNew;
      }
  
      public void setRankOldAndNew(String rankOldAndNew) {
          this.rankOldAndNew = rankOldAndNew;
      }
  
      public String getMovieCd() {
          return movieCd;
      }
  
      public void setMovieCd(String movieCd) {
          this.movieCd = movieCd;
      }
  
      public String getMovieNm() {
          return movieNm;
      }
  
      public void setMovieNm(String movieNm) {
          this.movieNm = movieNm;
      }
  
      public String getOpenDt() {
          return openDt;
      }
  
      public void setOpenDt(String openDt) {
          this.openDt = openDt;
      }
  
      public String getSalesAmt() {
          return salesAmt;
      }
  
      public void setSalesAmt(String salesAmt) {
          this.salesAmt = salesAmt;
      }
  
      public String getSalesShare() {
          return salesShare;
      }
  
      public void setSalesShare(String salesShare) {
          this.salesShare = salesShare;
      }
  
      public String getSalesInten() {
          return salesInten;
      }
  
      public void setSalesInten(String salesInten) {
          this.salesInten = salesInten;
      }
  
      public String getSalesChange() {
          return salesChange;
      }
  
      public void setSalesChange(String salesChange) {
          this.salesChange = salesChange;
      }
  
      public String getSalesAcc() {
          return salesAcc;
      }
  
      public void setSalesAcc(String salesAcc) {
          this.salesAcc = salesAcc;
      }
  
      public String getAudiCnt() {
          return audiCnt;
      }
  
      public void setAudiCnt(String audiCnt) {
          this.audiCnt = audiCnt;
      }
  
      public String getAudiInten() {
          return audiInten;
      }
  
      public void setAudiInten(String audiInten) {
          this.audiInten = audiInten;
      }
  
      public String getAudiChange() {
          return audiChange;
      }
  
      public void setAudiChange(String audiChange) {
          this.audiChange = audiChange;
      }
  
      public String getAudiAcc() {
          return audiAcc;
      }
  
      public void setAudiAcc(String audiAcc) {
          this.audiAcc = audiAcc;
      }
  
      public String getScrnCnt() {
          return scrnCnt;
      }
  
      public void setScrnCnt(String scrnCnt) {
          this.scrnCnt = scrnCnt;
      }
  
      public String getShowCnt() {
          return showCnt;
      }
  
      public void setShowCnt(String showCnt) {
          this.showCnt = showCnt;
      }
  }
  
  ```

  

- MainActivity

  - login 화면

    ```R
    package com.example.p533;
    
    import androidx.appcompat.app.AlertDialog;
    import androidx.appcompat.app.AppCompatActivity;
    
    import android.app.ProgressDialog;
    import android.content.Context;
    import android.content.DialogInterface;
    import android.content.Intent;
    import android.os.AsyncTask;
    import android.os.Bundle;
    import android.util.Log;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.AdapterView;
    import android.widget.BaseAdapter;
    import android.widget.LinearLayout;
    import android.widget.ListView;
    import android.widget.TextView;
    
    import com.google.gson.Gson;
    
    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;
    
    import java.util.ArrayList;
    
    public class MainActivity extends AppCompatActivity {
        TextView tx_id,tx_pwd;
        HttpAsync httpAsync;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            tx_id = findViewById(R.id.edit_id);
            tx_pwd = findViewById(R.id.edit_pwd);
        }
    
        public void clickbt(View v){
            String id = tx_id.getText().toString();
            String pwd = tx_pwd.getText().toString();
            String url = "http://192.168.219.106/android/login.jsp";
            url += "?id="+id+"&pwd="+pwd;
            httpAsync = new HttpAsync();
            httpAsync.execute(url);
        }
    
        class HttpAsync extends AsyncTask<String,String,String> {
            ProgressDialog progressDialog;
    
            @Override
            protected void onPreExecute() {
                progressDialog = new ProgressDialog(MainActivity.this);
                progressDialog.setTitle("Login ...");
                progressDialog.setCancelable(false);
                progressDialog.show();
            }
    
            @Override
            protected String doInBackground(String... strings) {
                String url = strings[0].toString();
                String result = HttpConnect.getString(url);
                return result;
            }
    
            @Override
            protected void onProgressUpdate(String... values) {
                super.onProgressUpdate(values);
            }
    
            @Override
            protected void onPostExecute(String s) {
                progressDialog.dismiss();
                String result = s.trim();
                if (result.equals("1")) {
                    // LOGIN COMPLETE
                    Intent intent = new Intent(MainActivity.this,
                            SecondActivity.class);
                    startActivity(intent);
                } else {
                    // LOGIN FAIL
                    androidx.appcompat.app.AlertDialog.Builder dailog = new AlertDialog.Builder(MainActivity.this);
                    dailog.setTitle("LOGIN FAIL");
                    dailog.setMessage("Try Again...");
                    dailog.setPositiveButton("ok", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialog, int which) {
                            return;
                        }
                    });
                    dailog.show();
                }
            }
        }
    }
    ```

    

- SecondActivity

  - 영화 리스트 출력

    ```java
    package com.example.p533;
    
    import androidx.appcompat.app.AppCompatActivity;
    
    import android.app.AlertDialog;
    import android.app.ProgressDialog;
    import android.content.Context;
    import android.content.DialogInterface;
    import android.os.AsyncTask;
    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.AdapterView;
    import android.widget.BaseAdapter;
    import android.widget.LinearLayout;
    import android.widget.ListView;
    import android.widget.TextView;
    
    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;
    
    import java.util.ArrayList;
    
    public class SecondActivity extends AppCompatActivity {
        ListView listView;
        TextView tx_term;
        LinearLayout container;
        ArrayList<Movie> list;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_second);
            listView = findViewById(R.id.listView);
            container = findViewById(R.id.container);
            tx_term = findViewById(R.id.tx_term);
            list = new ArrayList<>();
            getData();
        }
    
        private void getData(){
            String url = "http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=430156241533f1d058c603178cc3ca0e&targetDt=20201018";
            ItemAsync itemAsync = new ItemAsync();
            itemAsync.execute(url);
    
    
            listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
                @Override
                public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                    AlertDialog.Builder builder = new AlertDialog.Builder(SecondActivity.this);
                    builder.setTitle(list.get(i).getMovieNm());
                    builder.setMessage("박스 오피스 " + list.get(i).getRank() + "위 \n개봉일: " + list.get(i).getOpenDt()
                            + "\n상영영화관: " + list.get(i).getScrnCnt() + "\n관객수: " + list.get(i).getAudiCnt());
    
                    builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialogInterface, int i) {
                        }
                    });
    
                    builder.show();
                }
            });
        }
    
        class ItemAsync extends AsyncTask<String, Void, String> {
            ProgressDialog progressDialog;
    
            @Override
            protected void onPreExecute() {
                progressDialog = new ProgressDialog(SecondActivity.this);
                progressDialog.setTitle("get Data ...");
                progressDialog.setCancelable(false);
                progressDialog.show();
            }
    
            @Override
            protected String doInBackground(String... strings) {
                String url = strings[0].toString();
                String result = HttpConnect.getString(url);
                return result;
            }
    
            @Override
            protected void onProgressUpdate(Void... values) {
                super.onProgressUpdate(values);
            }
    
            @Override
            protected void onPostExecute(String s) {
                progressDialog.dismiss();
                String term = "";
                // 1. JSON으로 데이터 불러오기
                JSONArray ja = null;
                JSONObject jsonObject = null;
                try {
                    jsonObject = new JSONObject(s);
                    String str = jsonObject.getString("boxOfficeResult");
                    jsonObject = new JSONObject(str);
                    term = jsonObject.getString("showRange");
                    String str2 = jsonObject.getString("dailyBoxOfficeList");
    
                    ja = new JSONArray(str2);
                    for (int i = 0; i< ja.length(); i++){
                        JSONObject jo = ja.getJSONObject(i);
                        String name = jo.getString("movieNm");
                        String cnt = jo.getString("audiCnt");
                        String openDt = jo.getString("openDt");
                        String scrnCnt = jo.getString("scrnCnt");
                        String rank = jo.getString("rank");
                        Movie movie = new Movie(rank, name, openDt, cnt, scrnCnt);
                        list.add(movie);
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
    //            2. Gson으로 데이터 불러오기
    //            Gson gson = new Gson();
    //            MovieList movieList = gson.fromJson(s, MovieList.class);
    //
    //            for (int i = 0; i< movieList.boxOfficeResult.dailyBoxOfficeList.size(); i++){
    //                Movie movie = movieList.boxOfficeResult.dailyBoxOfficeList.get(i);
    //                list.add(movie);
    //            }
                tx_term.setText(term);
                ItemAdapter itemAdapter = new ItemAdapter();
                listView.setAdapter(itemAdapter);
            }
        } // end AsyncTask
    
        class ItemAdapter extends BaseAdapter {
    
            @Override
            public int getCount() {
                return list.size();
            }
    
            @Override
            public Object getItem(int i) {
                return list.get(i);
            }
    
            @Override
            public long getItemId(int i) {
                return i;
            }
    
            @Override
            public View getView(int i, View view, ViewGroup viewGroup) {
                View itemView = null;
                LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                itemView = inflater.inflate(R.layout.item, container, true);
    
                TextView tx_name = itemView.findViewById(R.id.textView);
                TextView tx_cnt = itemView.findViewById(R.id.textView2);
                tx_name.setText(list.get(i).getMovieNm());
                tx_cnt.setText(list.get(i).getAudiCnt() + " 명");
    
    
                return itemView;
            }
        }
    }
    ```



# 결과

![image-20201019175723876](C:%5CUsers%5CLSR%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201019175723876.png)
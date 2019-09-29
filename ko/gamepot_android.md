---
search:
  keyword: ['gamepot']
---

# 1. 시작하기

## 개발 환경 구성

Android용 애플리케이션의 개발을 위해서는 개발 툴(Android Studio 등)을 설치해야 합니다. 사용하는 개발 툴에 따라서는 추가적으로 Java SDK와 Android SDK 등을 설치해야 할 수도 있습니다.

Android에서 GAMEPOT을 사용하기 위한 시스템 환경은 다음과 같습니다.

[ 시스템 환경 ]

- 최소사항: API 17 (Jelly Bean, 4.2) 이상, gradle 2.3.0 이상
- 개발 환경: Android Studio

### 프로젝트 생성

![gamepot_android_01](./images/gamepot_android_01.png)

### 라이브러리 추가

다운로드한 AOS SDK 파일을 app/libs 폴더에 추가합니다.

![gamepot_android_01](./images/gamepot_android_02.png)

### build.gradle 설정

build.gradle 파일은 프로젝트 root 폴더와 app 폴더에 각각 존재합니다.

1. 프로젝트 root 폴더의 build.gradle 파일 수정

   ```java
   buildscript {

       repositories {
           ...
           google()
           jcenter()
           maven { url "https://jitpack.io" }
           maven { url "https://jcenter.bintray.com" }
       }
       dependencies {
          ...
           classpath 'com.google.gms:google-services:4.2.0'
       }
   }

   allprojects {
       repositories {
           ...
           google()
           jcenter()
           maven { url "https://jitpack.io" }
           maven { url "https://jcenter.bintray.com" }
       }
   }

   ```

2. app 폴더의 build.gradle 수정

   > [xxxxx]에는 실제 적용될 값을 넣습니다.

   | 값                           | 설명                                                         |
   | ---------------------------- | ------------------------------------------------------------ |
   | gamepot_project_id           | GAMEPOT에서 발급받은 프로젝트 아이디를 입력해 주세요.        |
   | gamepot_api_url              | GAMEPOT에서 발급받은 API URL을 입력해 주세요.                |
   | gamepot_store                | 스토어값(`google` 또는 `one`)                                |
   | gamepot_app_title            | 앱 제목 (FCM)                                                |
   | gamepot_push_default_channel | 등록된 기본 채널 이름 (Default) - 변경하지 마세요.           |
   | facebook_app_id              | 페이스북 발급 받은 앱ID                                      |
   | fb_login_protocol_scheme     | 페이스북에서 발급 받은 protocol scheme  fb[app_id]           |
   | gamepot_elsa_projectid       | NCLOUD ELSA 사용시 프로젝트ID ([자세히 보기](https://www.ncloud.com/product/analytics/elsa)) |

   ```java
   android {
       defaultConfig {
           ...
           // GamePot [START]
           resValue "string", "gamepot_project_id", "[projectId]" // required
           resValue "string", "gamepot_api_url", "[apiUrl]" // required
           resValue "string", "gamepot_store", "[storeId]" // required
           resValue "string", "gamepot_app_title","@string/app_name" // required (fcm)
           resValue "string", "gamepot_push_default_channel","Default" // required (fcm)
           resValue "string", "facebook_app_id", "[Facebook ID]" // facebook
           resValue "string", "fb_login_protocol_scheme", "fb[Facebook ID]" // (facebook)
           // resValue "string", "gamepot_elsa_projectid", "" // (ncp elsa)
           // GamePot [END]
       }
   }
   
   repositories {
       flatDir {
           dirs 'libs'
       }
   }
   
   dependencies {
       compile 'com.android.support:multidex:1.0.1'
   
       // GamePot common [START]
       compile(name: 'gamepot-common', ext: 'aar')
       compile('io.socket:socket.io-client:1.0.0') {
           exclude group: 'org.json', module: 'json'
       }
       compile('com.github.ihsanbal:LoggingInterceptor:3.0.0') {
           exclude group: 'org.json', module: 'json'
       }
       compile "com.github.nisrulz:easydeviceinfo:2.4.1"
       compile 'com.android.installreferrer:installreferrer:1.0'
       compile 'com.google.code.gson:gson:2.8.2'
       compile 'com.jakewharton.timber:timber:4.7.0'
       compile 'com.squareup.okhttp3:okhttp:3.10.0'
       compile 'com.apollographql.apollo:apollo-runtime:1.0.0-alpha2'
       compile 'com.apollographql.apollo:apollo-android-support:1.0.0-alpha2'
       compile 'com.android.billingclient:billing:1.1'
       compile 'com.github.bumptech.glide:glide:3.7.0'
       compile 'com.romandanylyk:pageindicatorview:1.0.0'
       compile 'com.google.firebase:firebase-core:16.0.6'
       compile 'com.google.firebase:firebase-messaging:17.3.4'
       // GamePot common [END]
   
       compile(name: 'gamepot-channel-base', ext: 'aar')
       // GamePot facebook [START]
       compile(name: 'gamepot-channel-facebook', ext: 'aar')
       compile 'com.facebook.android:facebook-android-sdk:5.2.0'
       // GamePot facebook [END]
   
       // GamePot google sigin [START]
       compile(name: 'gamepot-channel-google-signin', ext: 'aar')
       compile "com.google.android.gms:play-services-base:16.0.1"
       compile "com.google.android.gms:play-services-auth:16.0.1"
       // GamePot google sigin [END]
   }
   
   // ADD THIS AT THE BOTTOM
   apply plugin: 'com.google.gms.google-services'
   ```

3. 구글에서 발급받은 google-service.json 파일을 /app/ 폴더 하위에 복사합니다.

4. Gradle Sync Now

   Android Studio에서 아래 버튼을 클릭하여 새로고침합니다.

![gamepot_android_03](./images/gamepot_android_03.png)

* 새로고침을 누른 후 발생할 수 있는 실패

  * Configuration 'compile' is obsolete and has been replaced with 'implementation' and 'api'.
    It will be removed at the end of 2018. For more information see: <http://d.android.com/r/tools/update-dependency-configurations.html>

    > Gradle 버전을 3 이상 쓰시는 경우 compile을 implementation

  * No matching client found for package name 'packagename'

    > app의 패키지명과 google-service.json에 선언된 패키지명을 일치하도록 변경해주세요.

### AndroidManifest.xml 설정

일반적으로 게임에 사용되는 설정 값을 추가합니다. 각 설정별로 자세한 설명은 코드를 참고해주세요.

> 권장 사항으로 개발사 판단하에 적용 여부를 검토해주세요.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <!--전화 기능이 없는 기기(태블릿)에서도 스토어에서 다운로드할 수 있도록 설정-->
    <uses-feature android:name="android.hardware.telephony" android:required="false" />
    <!--음성 채팅이 지원되는 게임을 마이크가 없는 기기에서도 스토어에서 다운로드할 수 있도록 설정-->
    <uses-feature android:name="android.hardware.microphone" android:required="false" />

    <!--allowBackup을 필히 false로 해주세요. (게임이 재설치되면 자동으로 shared preference값을 복구하는 것을 막는 용도입니다.)-->
    <application
        android:name="android.support.multidex.MultiDexApplication"
        android:allowBackup="false"
        tools:replace="android:allowBackup">

        <!--resizeableActivity : 앱 분할 화면 보기 기능 비활성화-->
        <activity
            android:resizeableActivity="false">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!--갤럭시 S8과 같은 스크린 대응-->
        <meta-data android:name="android.max_aspect" android:value="2.1" />

    </application>
</manifest>
```

### Push Notification 아이콘 설정

![gamepot_android_04](./images/gamepot_android_04.png)

푸시 수신 시 Notification bar에 보여줄 icon은 기본적으로 SDK 내부의 기본 이미지로 처리되며, 게임에 맞게 직접 넣을 수도 있습니다.

#### icon 직접 넣기

> [Android Asset Studio](http://romannurik.github.io/AndroidAssetStudio/icons-notification.html#source.type=clipart&source.clipart=ac_unit&source.space.trim=1&source.space.pad=0&name=ic_stat_gamepot_small)를 사용하여 아이콘을 제작하면 자동으로 폴더별로 제작되므로 각 폴더에 넣기만 하면 됩니다.

1. res/drawable 관련 폴더를 아래와 같이 생성
   - res/drawable-mdpi/
   - res/drawable-hdpi/
   - res/drawable-xhdpi/
   - res/drawable-xxhdpi/
   - res/drawable-xxxhdpi/

2. 아래 사이즈별로 이미지 제작
    - 24x24
    - 36x36
    - 48x48
    - 72x72
    - 96x96

3. 아래와 같이 각 폴더별로 사이즈에 맞는 이미지를 추가

|  폴더명                 |  사이즈  |
|  --------------------  |  -----  |
|  res/drawable-mdpi/     |  24x24  |
|  res/drawable-hdpi/     |  36x36  |
|  res/drawable-xhdpi/    |  48x48  |
|  res/drawable-xxhdpi/   |  72x72  |
|  res/drawable-xxxhdpi/  |  96x96  |

4. 이미지 파일명을 `ic_stat_gamepot_small`로 변경

# 2. 초기화

MainActivity.java 파일에 아래 부분을 추가합니다.

```java
import io.gamepot.common.GamePot;
import io.gamepot.common.GamePotLocale;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    // GAMEPOT 초기화. context는 꼭 application context를 넣어 주세요.
    // setup API는 다른 API보다 가장 처음에 호출해야 합니다.
    GamePot.getInstance().setup(getApplicationContext());
}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    GamePot.getInstance().onActivityResult(requestCode, resultCode, data);
}

@Override
protected void onStart() {
    super.onStart();
    GamePotChat.getInstance().start();
}

@Override
protected void onStop() {
    super.onStop();
    GamePotChat.getInstance().stop();
}

@Override
protected void onDestroy() {
    super.onDestroy();
    GamePot.getInstance().onDestroy();
}
```

# 3. 로그인, 로그아웃, 회원 탈퇴

구글, 페이스북, 네이버 등 다양한 로그인 SDK를 통합하여 사용할 수 있습니다.

## 구글(Firebase) 콘솔 설정

APK 빌드 시 사용한 Keystore의 SHA-1 값을 Firebase console에 추가합니다.

> SHA-1 값은 개발사에 요청합니다.

![gamepot_android_05](./images/gamepot_android_05.png)

## 페이스북 콘솔 설정

APK 빌드 시 사용한 Keystore의 키 해시 값을 페이스북 콘솔에 추가합니다.

> 키 해시 값은 개발사에 요청합니다.

![gamepot_android_06](./images/gamepot_android_06.png)

## 설정

### MainActivity.java 파일 수정

로그인 관련 코드를 아래와 같이 선언합니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.facebook.GamePotFacebook;
import io.gamepot.channel.google.signin.GamePotGoogleSignin;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // setup API는 맨 처음에 호출돼야 합니다.
        GamePot.getInstance().setup(getApplicationContext());

        ...
        // 로그인을 사용하려는 채널별로 addChannel을 호출해주세요.(Guest 방식은 기본으로 포함)
        // Google Login 초기화
        GamePotChannel.getInstance().addChannel(this, GamePotChannelType.GOOGLE, new GamePotGoogleSignin());
        // Facebook Login 초기화
        GamePotChannel.getInstance().addChannel(this, GamePotChannelType.FACEBOOK, new GamePotFacebook());
        ...
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        GamePotChannel.getInstance().onActivityResult(this, requestCode, resultCode, data);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        GamePotChannel.getInstance().onDestroy();
    }
}
```

## 로그인

로그인 UI는 개발사에서 구현하고, 로그인 버튼 클릭 시에 연동합니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelListener;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.GamePotUserInfo;
import io.gamepot.common.GamePotError;

// 로그인 타입 정의
// GamePotChannelType.GOOGLE: 구글
// GamePotChannelType.FACEBOOK: 페이스북
// GamePotChannelType.NAVER: 네이버
// GamePotChannelType.LINE: 라인
// GamePotChannelType.TWITTER: 트위터
// GamePotChannelType.GUEST: 게스트

// 구글 로그인 버튼을 눌렀을 때 호출
GamePotChannel.getInstance().login(this, GamePotChannelType.GOOGLE, new GamePotChannelListener<GamePotUserInfo>() {
    @Override
    public void onCancel() {
        // 사용자가 로그인을 취소한 상황.
    }

    @Override
    public void onSuccess(GamePotUserInfo userinfo) {
        // 로그인 완료. 게임 로직에 맞게 처리해주세요.
        // userinfo.getMemberid() : 회원 고유 아이디
    }

    @Override
    public void onFailure(GamePotError error) {
        // 로그인 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

### 회원 고유 아이디

```java
GamePot.getInstance().getMemberId();
```

## 자동 로그인

사용자가 마지막에 로그인 했던 정보를 전달하는 API를 이용하여 자동 로그인을 구현할 수 있습니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelListener;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.GamePotUserInfo;
import io.gamepot.common.GamePotError;

// 사용자가 마지막에 로그인했던 정보를 전달하는 API
final GamePotChannelType lastLoginType = GamePotChannel.getInstance().getLastLoginType();

if(lastLoginType != GamePotChannelType.NONE) {
    // 마지막에 로그인했던 로그인 타입으로 로그인하는 방식입니다.
    GamePotChannel.getInstance().login(this, lastLoginType, new GamePotChannelListener<GamePotUserInfo>() {
        @Override
        public void onCancel() {
            // 사용자가 로그인을 취소한 상황.
        }

        @Override
        public void onSuccess(GamePotUserInfo info) {
            // 자동 로그인 완료. 게임 로직에 맞게 처리해주세요.
        }

        @Override
        public void onFailure(GamePotError error) {
            // 자동 로그인 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
        }
    });
}
else
{
    // 처음 게임을 실행했거나 로그아웃한 상태. 로그인을 할 수 있는 로그인 화면으로 이동해주세요.
}
```

## 로그아웃

현재 회원 계정을 로그아웃합니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.common.GamePotCommonListener;
import io.gamepot.common.GamePotError;

GamePotChannel.getInstance().logout(this, new GamePotCommonListener() {
    @Override
    public void onSuccess() {
        // 로그아웃 완료. 초기 화면으로 이동해주세요.
    }

    @Override
    public void onFailure(GamePotError error) {
        // 로그아웃 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

## 회원 탈퇴

현재 회원 계정을 탈퇴시킵니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.common.GamePotCommonListener;
import io.gamepot.common.GamePotError;

GamePotChannel.getInstance().deleteMember(this, new GamePotCommonListener() {
    @Override
    public void onSuccess() {
        // 회원탈 퇴 성공. 초기화면으로 이동해주세요.
    }

    @Override
    public void onFailure(GamePotError error) {
        // 회원 탈퇴 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

##검증

로그인 완료 후 로그인 정보를 개발사 서버에서 GAMEPOT 서버로 전달하면 로그인 검증이 진행됩니다.

자세한 설명은 `Server to server api` 매뉴에 `Authentication check` 항목을 참고해주세요.

# 4. 계정 연동

하나의 게임 계정에 복수 개의 소셜 계정(구글, 페이스북 등)을 연결/해제할 수 있는 기능입니다.(최소 연동 소셜 계정은 1가지입니다.)

> 연동화면 UI는 개발사에서 구현해주세요.

## 계정 연동

Google, Facebook 등의 아이디로 계정을 연동할 수 있습니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelListener;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.GamePotUserInfo;
import io.gamepot.common.GamePotError;

// 구글 계정에 연동
// GamePotChannelType.GOOGLE
// 페이스북 계정에 연동
// GamePotChannelType.FACEBOOK
// 네이버 계정에 연동
// GamePotChannelType.NAVER
// 라인 계정에 연동
// GamePotChannelType.LINE
// 트위터 계정에 연동
// GamePotChannelType.TWITTER

GamePotChannel.getInstance().createLinking(this, GamePotChannelType.GOOGLE, new GamePotChannelListener<GamePotUserInfo>() {
    @Override
    public void onSuccess(GamePotUserInfo userInfo) {
        // 연동 완료. 연동 결과에 대한 문구를 노출시켜 주세요.(예: 계정 연동에 성공했습니다.)
    }

    @Override
    public void onCancel() {
        // 사용자가 취소한 경우
    }

    @Override
    public void onFailure(GamePotError error) {
        // 연동 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

## 연동된 리스트

해당 API를 통해 계정에 대해 연동 여부를 확인하실 수 있습니다.

```java
import io.gamepot.channel.GamePotChannel;
import java.util.ArrayList;

// 타입 정의
// GamePotChannelType.GOOGLE
// GamePotChannelType.FACEBOOK
// GamePotChannelType.NAVER
// GamePotChannelType.LINE
// GamePotChannelType.TWITTER
// 타입에 따른 연동 결과를 반환합니다.
boolean isLinked = GamePotChannel.getInstance().isLinked(GamePotChannelType.GOOGLE);

// 연동되어 있는 모든 타입에 대해 JSON 형태로 반환합니다.
// 만약 GOOGLE과 FACEBOOK에 연동된 경우 아래와 같이 반환됩니다.
// [{“provider”:”google”},{“provider”:”facebook”}]
JSONArray linking = GamePotChannel.getInstance().getLinkedList();
```

## 연동 해제

기존에 연동되어 있는 계정을 해제합니다.

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.common.GamePotCommonListener;
import io.gamepot.common.GamePotError;

GamePotChannel.getInstance().deleteLinking(this, GamePotChannelType.GOOGLE, new GamePotCommonListener() {
    @Override
    public void onSuccess() {
        // 연동 해제 완료. 연동 결과에 대한 문구를 노출시켜 주세요. (예: 계정 연동을 해지했습니다.)
    }

    @Override
    public void onFailure(GamePotError error) {
        // 연동 해제 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

# 5. 결제

결제의 결과 값은 Listener 형태로 구현되어 있습니다.

MainActivity.java에서 앱 실행 시 한 번 호출하도록 선언합니다.

```java
import io.gamepot.common.GamePot;
import io.gamepot.common.GamePotPurchaseInfo;
import io.gamepot.common.GamePotPurchaseListener;
import io.gamepot.common.GamePotError;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // setup API는 맨 처음에 호출돼야 합니다.
        GamePot.getInstance().setup(getApplicationContext());

        ...
		GamePot.getInstance().setPurchaseListener(new GamePotPurchaseListener<GamePotPurchaseInfo>() {
            @Override
            public void onSuccess(GamePotPurchaseInfo info) {
                // 결제 성공. 아이템 지급 요청은 webhook에 설정된 주소로 server to server로 요청합니다.
                // 이 곳에서는 결과에 대한 처리만 해주시고 실제 아이템 지급은 하지 마세요.
            }

            @Override
            public void onFailure(GamePotError error) {
                // 결제 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
            }

            @Override
            public void onCancel() {
                // 결제 진행 중 사용자가 취소한 경우
            }
        });
        ...
    }
}
```

## 결제 시도

하나의 결제 API로 GooglePlay, OneStore 모두 결제가 가능합니다.

> 결제 시도 ~ 결제 완료/실패 과정중에 인게임에서 사용하는 로딩 화면을 띄워 중복 호출을 하지 않도록 처리해주세요.

```java
import io.gamepot.common.GamePot;

// productId는 스토어에 등록된 상품ID를 입력해 주시면 됩니다.
GamePot.getInstance().purchase("product id");
```

## 결제 아이템 리스트 획득

스토어에서 전달하는 인앱 아이템 리스트를 획득할 수 있습니다.

```java
import io.gamepot.common.GamePot;

GamePotPurchaseDetailList details = GamePot.getInstance().getPurchaseDetailList();
```

##결제 아이템 지급

GAMEPOT은 Server to server api를 통해 결제 스토어에 영수증 검증까지 모두 마친 후 개발사 서버에 지급 요청을 하기 때문에 불법 결제가 불가능합니다.

이를 위해선 `Server to server api` 메뉴에 `Purchase` 항목을 참고하여 처리하셔야 합니다.

# 6. 외부결제

원스토어의 경우 기본 스토어 결제 모듈이 아닌 제 3의 결제모듈을 허용하고 있습니다.

## 설정

대시보드에 외부결제 항목을 참고하여 대시보드 설정을 먼저 진행하세요.

`5. 결제` 항목을 먼저 구현했다면 추가로 설정할 부분은 없습니다.

## 결제 시도

```java
import io.gamepot.common.GamePot;

// activity : 현재 액티비티
// product id : 대시보드에 등록한 결제 아이디
GamePot.getInstance().purchaseThirdPayments(activity, product id);
```

## 결제 아이템 리스트 획득

```java
import io.gamepot.common.GamePot;

GamePotPurchaseDetailList thirdPaymentsDetailList = GamePot.getInstance().getPurchaseThirdPaymentsDetailList();
```

# 7. 기타 API

##네이버 로그인

###build.gradle 설정

```java
android {
    defaultConfig {
        ...
        resValue "string", "gamepot_naver_clientid", "xxxxxxxx" // Naver 개발자 콘솔에서 획득
        resValue "string", "gamepot_naver_secretid", "xxx" // Naver 개발자 콘솔에서 획득
    }
}

dependencies {
  ...
  compile(name: 'gamepot-channel-naver', ext: 'aar')
  ...
}
```

### MainActivity.java 설정

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.naver.GamePotNaver;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
		...
		GamePotChannel.getInstance().addChannel(this, GamePotChannelType.NAVER, new GamePotNaver());
}
```

### 로그인

```java
GamePotChannel.getInstance().login(this, GamePotChannelType.NAVER, new GamePotAppStatusChannelListener<GamePotUserInfo>() {
  ...
});
```

## 라인 로그인

### build.gradle 설정

```java
android {
    defaultConfig {
        ...
        resValue "string", "gamepot_line_channelid","00000000" // Line 개발자 콘솔에서 획득
    }
}

dependencies {
  ...
  compile(name: 'gamepot-channel-line', ext: 'aar')
  compile(name: 'line-sdk-4.0.10', ext: 'aar')
  ...
}
```

### MainActivity.java 설정

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.line.GamePotLine;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
		...
		GamePotChannel.getInstance().addChannel(this, GamePotChannelType.LINE, new GamePotLine());
}
```

### 로그인

```java
GamePotChannel.getInstance().login(this, GamePotChannelType.LINE, new GamePotAppStatusChannelListener<GamePotUserInfo>() {
  ...
});
```

## 트위터 로그인

### build.gradle 설정

```java
android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
  
    defaultConfig {
        ...
        resValue "string", "gamepot_twitter_consumerkey","xxxxx" // Twitter 개발자 콘솔에서 획득
        resValue "string", "gamepot_twitter_consumersecret","xxx" // Twitter 개발자 콘솔에서 획득
    }
}

dependencies {
  ...
  compile(name: 'gamepot-channel-twitter', ext: 'aar')
  compile('com.twitter.sdk.android:twitter-core:3.3.0@aar') {
      transitive = true
  }
  ...
}
```

### MainActivity.java 설정

```java
import io.gamepot.channel.GamePotChannel;
import io.gamepot.channel.GamePotChannelType;
import io.gamepot.channel.twitter.GamePotTwitter;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
		...
		GamePotChannel.getInstance().addChannel(this, GamePotChannelType.TWITTER, new GamePotTwitter());
}
```

### 로그인

```java
GamePotChannel.getInstance().login(this, GamePotChannelType.TWITTER, new GamePotAppStatusChannelListener<GamePotUserInfo>() {
  ...
});
```

## 쿠폰

사용자에게 입력받은 쿠폰을 사용할 때 아래 코드를 호출해 주세요.

> 쿠폰 입력 화면 UI는 개발사에서 구현해주세요.

```java
import io.gamepot.common.GamePot;
import io.gamepot.common.GamePotError;
import io.gamepot.common.GamePotListener;

GamePot.getInstance().coupon(/*사용자에게 입력받은 쿠폰*/, new GamePotListener<String>() {
    @Override
    public void onSuccess(String message) {
        // 쿠폰 사용 성공. message값을 팝업으로 노출해주세요.
    }

    @Override
    public void onFailure(GamePotError error) {
        // 쿠폰 사용 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

###아이템 지급

쿠폰 사용이 성공하면 개발사 서버에 Server to server api를 통해 아이템 지급을 요청합니다.

이를 위해선 `Server to server api` 메뉴에 `Item` 항목을 참고하여 처리하셔야 합니다.

## Push on/off

전체푸시, 야간푸시, 광고성푸시 3가지 종류의 푸시를 각각 on/off를 처리 할 수 있습니다.

> on/off 설정하는 UI는 개발사에서 구현해주세요.

```java
import io.gamepot.common.GamePot;
import io.gamepot.common.GamePotError;
import io.gamepot.common.GamePotCommonListener;

// 푸시 수신 On/Off
GamePot.getInstance().setPushEnable(/*true or false*/, new GamePotCommonListener() {
    @Override
    public void onSuccess() {
    }

    @Override
    public void onFailure(GamePotError error) {
    }
});

// 야간 푸시 수신 On/Off
GamePot.getInstance().setNightPushEnable(/*true or false*/, new GamePotCommonListener() {
    @Override
    public void onSuccess() {
    }

    @Override
    public void onFailure(GamePotError error) {
    }
});

// 푸시, 야간푸시를 한번에 설정
// 로그인 전에 푸시, 야간푸시 허용 여부를 받는 게임이라면 로그인 후에 아래 코드로 필히 호출합니다.
GamePot.getInstance().setPushEnable(/*true or false*/, /*true or false*/, true, new GamePotCommonListener() {
    @Override
    public void onSuccess() {
    }

    @Override
    public void onFailure(GamePotError error) {
    }
});
```

현재 푸시 상태를 가져오려면 아래 코드를 참고하세요.

```java
import io.gamepot.common.GamePot;
import org.json.JSONObject;

// enable: 전체푸시
// night: 야간푸시
// {"enable":true, "night":true}
JSONObject status = GamePot.getInstance().getPushStatus();
```

## 공지사항

대시보드 - 공지사항에서 업로드한 이미지가 노출되는 기능입니다.

### 호출

```java
GamePot.getInstance().showNotice(/*현재 액티비티*/, new GamePotNoticeDialog.onSchemeListener() {
    @Override
    public void onReceive(String scheme) {
        // TODO : scheme 처리
    }
});
```

## 고객센터

대시보드 - 고객센터와 연동되는 유저와 운영자간에 소통 채널입니다.

### 호출

```java
GamePot.getInstance().showCSWebView(/*현재 액티비티*/);
```

## 로컬 푸시(Local Push notification)

푸시 서버를 통하지 않고 단말기에서 자체적으로 푸시를 노출하는 기능입니다.

### 호출

#### 푸시 등록

정해진 시간에 로컬 푸시를 노출하는 방법은 아래와 같습니다.

> 리턴 값으로 전달되는 pushid는 개발사에서 관리합니다.

```java
String date = "2018-09-27 20:00:00";
GamePotLocalPushBuilder builder = new GamePotLocalPushBuilder(getActivity())
                        .setTitle("로컬푸시 테스트")
                        .setMessage("로컬푸시 메시지입니다. " + date)
                        .setDateString(date).build();
int pushid = GamePot.getInstance().sendLocalPush(builder);
```

#### 등록한 푸시 취소

푸시 등록 시 얻은 pushid를 기반으로 기존에 등록된 푸시를 취소할 수 있습니다.

```java
GamePot.getInstance().cancelLocalPush(/*현재 액티비티*/, /*푸시 등록시 얻은 pushid*/);
```

## 점검, 강제 업데이트

점검이나 강제 업데이트 기능이 필요한 경우 대시보드 - 운영에서 기능을 활성화할 경우 동작합니다.

### 호출

기존에 적용된 아래 API에서 사용이 가능합니다.

#### 1. login API

기존 login API에서 listener를 `GamePotAppStatusChannelListener`로 변경합니다.

```java
GamePotChannel.getInstance().login(this, GamePotChannelType.GOOGLE, new GamePotAppStatusChannelListener<GamePotUserInfo>() {
    @Override
    public void onNeedUpdate(GamePotAppStatus status) {
        // TODO: 강제 업데이트가 필요한 경우. 아래 API를 호출하면 SDK 자체에서 팝업을 띄울 수 있습니다.
        // TODO: Customizing을 하고자 하는 경우 아래 API를 호출하지 말고 Customizing을 하면 됩니다.
        GamePot.getInstance().showAppStatusPopup(MainActivity.this, status, new GamePotAppCloseListener() {
            @Override
            public void onClose() {
                // TODO: showAppStatusPopup API를 호출하신 경우 앱을 종료해야 하는 상황에 호출됩니다.
                // TODO: 종료 프로세스를 처리해주세요.
                MainActivity.this.finish();
            }

            @Override
            public void onNext(Object obj) {
                // TODO : Dashboard 업데이트 설정에서 권장 설정 시 "다음에 하기" 버튼이 노출 됩니다.
                // 해당 버튼을 사용자가 선택 시 호출 됩니다.
                // TODO : obj 정보를 이용하여 로그인 완료 시와 동일하게 처리해주세요.
                // GamePotUserInfo userInfo = (GamePotUserInfo)obj;
            }
        });
    }

    @Override
    public void onMainternance(GamePotAppStatus status) {
        // TODO: 점검 중인 경우. 아래 API를 호출하면 SDK 자체에서 팝업을 띄울 수 있습니다.
        // TODO: Customizing을 하고자 하는 경우 아래 API를 호출하지 말고 Customizing을 하면 됩니다.
        GamePot.getInstance().showAppStatusPopup(MainActivity.this, status, new GamePotAppCloseListener() {
            @Override
            public void onClose() {
                // TODO: showAppStatusPopup API를 호출하신 경우 앱을 종료해야 하는 상황에 호출됩니다.
                // TODO: 종료 프로세스를 처리해주세요.
                MainActivity.this.finish();
            }
        });
    }

    @Override
    public void onCancel() {
        // 사용자가 로그인을 취소한 상황.
    }

    @Override
    public void onSuccess(GamePotUserInfo userinfo) {
        // 로그인 완료. 게임 로직에 맞게 처리해주세요.
    }

    @Override
    public void onFailure(GamePotError error) {
        // 로그인 실패. error.getMessage()를 이용해서 오류 메시지를 보여주세요.
    }
});
```

## 약관 동의

'이용약관' 및 '개인정보 수집 및 이용안내' 동의를 쉽게 받을 수 있도록 UI를 제공합니다.

`BLUE` 테마와  `GREEN` 테마 두 가지를 제공하며, 각 영역별로 Customizing도 가능합니다.

- `BLUE` 테마 예시

  ![gamepot_unity_10](./images/gamepot_unity_10.png)

- `GREEN` 테마 예시

  ![gamepot_unity_11](./images/gamepot_unity_11.png)

### 약관 동의 호출

> 약관 동의 팝업 노출 여부는 개발사에서 게임에 맞게 처리해주세요.
>
> '보기'버튼을 클릭 시 보여지는 내용은 대시보드에서 적용 및 수정이 가능합니다.

Request:

```csharp
// 기본 호출(BLUE 테마로 적용)
GamePot.getInstance().showAgreeDialog(/*activity*/, new GamePotAgreeBuilder(), new GamePotListener<GamePotAgreeInfo>() {
    @Override
    public void onSuccess(GamePotAgreeInfo data) {
        // data.agree : 필수 약관을 모두 동의한 경우 true
        // data.agreeNight : 야간 광고성 수신 동의를 체크한 경우 true, 그렇지 않으면 false
        // agreeNight 값은 로그인 완료 후 setPushNightStatus api를 통해 전달하세요.
    }

    @Override
    public void onFailure(GamePotError error) {
	    // error.message를 팝업 등으로 유저에게 알려주세요.
    }
});

// GREEN 테마로 적용시
GamePotAgreeBuilder bulider = new GamePotAgreeBuilder(GamePotAgreeBuilder.THEME.GREEN);
GamePot.getInstance().showAgreeDialog(/*activity*/, bulider, new GamePotListener<GamePotAgreeInfo>() {
  ....
}
```

### Customizing

테마를 사용하지 않고 게임에 맞게 색을 변경합니다.

약관 동의를 호출하기 전에 `GamePotAgreeBuilder`에서 각 영역별로 색을 지정할 수 있습니다.

```c#
GamePotAgreeBuilder agreeBuilder= new GamePotAgreeBuilder();
agreeBuilder.setHeaderBackGradient(new int[] {0xFF00050B,0xFF0F1B21});
agreeBuilder.setHeaderTitleColor(0xFFFF0000);
agreeBuilder.setHeaderBottomColor(0xFF00FF00);
// 미사용시 ""로 설정
agreeBuilder.setHeaderTitle("약관 동의");
// res/drawable 객체 아이디
agreeBuilder.setHeaderIconDrawable(R.drawable.ic_stat_gamepot_agree);

agreeBuilder.setContentBackGradient(new int[] { 0xFFFF2432, 0xFF11FF32 });
agreeBuilder.setContentTitleColor(0xFF0429FF);
agreeBuilder.setContentCheckColor(0xFFFFADB5);
agreeBuilder.setContentIconColor(0xFF98FFC6);
agreeBuilder.setContentShowColor(0xFF98B3FF);
// res/drawable 객체 아이디
agreeBuilder.setContentIconDrawable(R.drawable.ic_stat_gamepot_small);

agreeBuilder.setFooterBackGradient(new int[] { 0xFFFFFFFF, 0xFF112432 });
agreeBuilder.setFooterButtonGradient(new int[] { 0xFF1E3A57, 0xFFFFFFFF });
agreeBuilder.setFooterButtonOutlineColor(0xFFFF171A);
agreeBuilder.setFooterTitleColor(0xFFFF00D5);
agreeBuilder.setFooterTitle("게임 시작하기");
// 야간 광고성 수신동의 버튼 노출 여부
agreeBuilder.setShowNightPush(true);

// 문구 변경
agreeBuilder.setAllMessage("모두 동의");
agreeBuilder.setTermMessage("필수) 이용약관");
agreeBuilder.setPrivacyMessage("필수) 개인정보 취급 방침");
agreeBuilder.setNightPushMessage("선택) 야간 푸시 수신 동의");

GamePot.getInstance().showAgreeDialog(/*activity*/, agreeBuilder, new GamePotListener<GamePotAgreeInfo>() {
  ....
}
```

각각의 변수는 아래 영역에 적용됩니다.

> contentIconDrawable의 기본 이미지는 푸시 아이콘으로 설정됩니다.

![gamepot_unity_12](./images/gamepot_unity_12.png)

## 이용약관

이용약관 UI를 호출합니다.

> 대시보드 - 고객지원 - 이용약관 설정 항목에 내용을 먼저 입력하세요.

```java
import io.gamepot.common.GamePot;

// activity : 현재 액티비티
GamePot.getInstance().showTerms(activity);
```

## 개인정보 취급방침

개인정보 취급방침 UI를 호출합니다.

> 대시보드 - 고객지원 - 개인정보취급방침 설정 항목에 내용을 먼저 입력하세요.

```java
import io.gamepot.common.GamePot;

// activity : 현재 액티비티
GamePot.getInstance().showPrivacy(activity);
```


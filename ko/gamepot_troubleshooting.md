# Troubleshooting

## Unity plugin

### Unity 2018.4.4이상, Unity 2019.2.0이상에서 Android 빌드 이슈

####1. `mainTemplate.gradle` 파일을 아래와 같이 수정

> TODO 항목을 참고하세요.

```java
// TODO : GradleVersion이 사용되는 곳을 모두 제거합니다.

buildscript {
	repositories {
		// if (GradleVersion.current() >= GradleVersion.version("4.2")) {
            google()
            jcenter()
        // } else {
        //     jcenter()
        // }
    }
    dependencies {
		// if (GradleVersion.current() < GradleVersion.version("4.0")) {
        //     classpath 'com.android.tools.build:gradle:2.1.0'
        // } else if (GradleVersion.current() < GradleVersion.version("4.2")) {
        //     classpath 'com.android.tools.build:gradle:2.3.0'
        // } else {
			      // TODO : Android gradle plugin version을 3.4.0버전으로 변경합니다.
            classpath 'com.android.tools.build:gradle:3.4.0'
        // }
		classpath 'com.google.gms:google-services:3.2.0'
	}
}

allprojects {
   repositories {
		flatDir {
			dirs 'libs'
		}

		// if (GradleVersion.current() >= GradleVersion.version("4.2")) {
            google()
            jcenter()
        // } else {
        //     jcenter()
        // }
   }
}


dependencies {
	// if (GradleVersion.current() >= GradleVersion.version("4.2")) {
		implementation fileTree(include: ['*.jar'], dir: 'libs')
		implementation project(":GamePotResources")
		implementation project(':Firebase')
	// } else {
	// 	compile fileTree(include: ['*.jar'], dir: 'libs')
	// 	compile project(":GamePotResources")
	// 	compile project(':Firebase')
	// }
}

fileTree(dir: 'libs', include: ['*.aar'])
        .each { File file ->
    // println file.name
	// if (GradleVersion.current() >= GradleVersion.version("4.2")) {
		dependencies.add("implementation", [name: file.name.lastIndexOf('.').with { it != -1 ? file.name[0..<it] : file.name }, ext: 'aar'])
	// } else {
    // 	dependencies.add("compile", [name: file.name.lastIndexOf('.').with { it != -1 ? file.name[0..<it] : file.name }, ext: 'aar'])
	// }
}
```

####2. Firebase 관련 파일을 수정

1. [링크](https://kr.object.ncloudstorage.com/gamepot/Firebase_patch.zip)를 통해 패치파일을 다운로드
2. 아래와 같이 파일 복사

```java
/Firebase_patch/Assets/Firebase/Editor
위 경로의 파일을 아래 경로에 복사
{unity project}/Assets/Firebase/Editor

{unity project}/Assets/PlayServicesResolver/Editor
위 경로의 파일을 모두 삭제 후 아래 경로에 파일을 복사
/Firebase_patch/Assets/PlayServicesResolver/Editor
```

3. /Assets/Plugins/Android/Firebase/res 폴더가 생성되지 않았다면 Unity 재실행
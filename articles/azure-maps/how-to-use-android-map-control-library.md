---
title: Azure Maps에서 Android 맵 컨트롤 시작 Microsoft Docs
description: Azure Maps의 Android 맵 컨트롤입니다.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 934fe2219ccca917999cf49cb9c9826276545e73
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70915668"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Azure Maps Android SDK 시작

Azure Maps Android SDK는 Android 용 벡터 맵 라이브러리입니다. 이 문서에서는 Azure Maps Android SDK를 설치 하 고 지도를 로드 하는 프로세스를 안내 합니다.

## <a name="prerequisites"></a>필수 구성 요소

### <a name="create-an-azure-maps-account"></a>Azure Maps 계정 만들기

이 문서의 절차를 완료 하려면 먼저 S1 가격 책정 계층에서 [Azure Maps 계정을 만들어야](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account) 합니다.

### <a name="download-android-studio"></a>Android Studio 다운로드

Azure Maps Android SDK를 설치 하기 전에 Android Studio을 다운로드 하 고 빈 작업으로 프로젝트를 만듭니다. Google에서 무료로 [Android Studio를 다운로드할](https://developer.android.com/studio/) 수 있습니다. 

## <a name="create-a-project-in-android-studio"></a>Android Studio에서 프로젝트 만들기

먼저 빈 작업을 사용 하 여 새 프로젝트를 만듭니다. Android Studio 프로젝트를 만들려면 다음 단계를 완료 합니다.

1. **프로젝트 선택**에서 **휴대폰 및 태블릿**을 선택 합니다. 응용 프로그램이이 폼 팩터로 실행 됩니다.
2. **휴대폰 및 태블릿** 탭에서 **빈 작업**을 선택 하 고 **다음**을 선택 합니다.
3. **프로젝트 구성에서** 최소 SDK로 `API 21: Android 5.0.0 (Lollipop)`을 선택합니다. 이 버전은 Azure Maps Android SDK에서 지 원하는 가장 빠른 버전입니다.
4. 기본값 `Activity Name` 을 `Layout Name` 그대로 적용 하 고 **마침**을 선택 합니다.

Android Studio를 설치 하 고 새 프로젝트를 만드는 방법에 대 한 자세한 내용은 [Android Studio 설명서](https://developer.android.com/studio/intro/) 를 참조 하세요.

![프로젝트 만들기](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>가상 디바이스 설정

Android Studio를 사용하면 컴퓨터에 가상 Android 디바이스를 설정할 수 있습니다. 이렇게 하면 개발 하는 동안 응용 프로그램을 테스트 하는 데 도움이 됩니다. 가상 장치를 설정 하려면 프로젝트 화면의 오른쪽 위에 있는 AVD (Android 가상 장치) 관리자 아이콘을 선택 하 고 **가상 장치 만들기**를 선택 합니다. 도구 모음에서 **도구** > **Android** > **avd 관리자** 를 선택 하 여 avd manager로 이동할 수도 있습니다. **전화** 범주에서 **Nexus 5**를 선택 하 고 **다음**을 선택 합니다.

[Android Studio 설명서](https://developer.android.com/studio/run/managing-avds)에서 avd를 설정 하는 방법에 대해 자세히 알아볼 수 있습니다.

![Android Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Azure Maps Android SDK 설치

응용 프로그램을 빌드하는 다음 단계는 Azure Maps Android SDK를 설치 하는 것입니다. SDK를 설치 하려면 다음 단계를 완료 합니다.

1. 최상위 **gradle** 파일을 열고 **모든 프로젝트**, **리포지토리** 블록 섹션에 다음 코드를 추가 합니다.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. **App/gradle** 를 업데이트 하 고 다음 코드를 추가 합니다.
    
    1. 프로젝트의 **매니페스트의 minsdkversion** 가 API 21 이상에 있는지 확인 합니다.

    2. Android 섹션에 다음 코드를 추가 합니다.

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. 종속성 블록을 업데이트 하 고 최신 Azure Maps Android SDK에 대 한 새 구현 종속성 줄을 추가 합니다.

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

    > [!Note]
    > Azure Maps Android SDK 정기적으로 업그레이드 되 고 향상 됩니다. [Android 맵 컨트롤 시작](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) 설명서를 참조 하 여 최신 Azure Maps 구현 버전 번호를 가져올 수 있습니다. 또한 버전 번호를 "0.2"에서 "0 +"로 설정 하 여 항상 최신 버전을 가리키도록 할 수 있습니다.

3. **Res** > layoutactivity_main > 을 편집 하 고 코드로 바꿉니다.
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    </FrameLayout>
    ```

4. **Mainactivity. java** 파일에서 다음을 수행 해야 합니다.
    
    * Azure Maps SDK에 대 한 가져오기 추가
    * Azure Maps 인증 정보 설정
    * **onCreate** 메서드에서 map 컨트롤 인스턴스 가져오기

    또는 `AzureMaps` `setSubscriptionKey` 메서드를 사용 하 여 클래스의 인증 정보를 전역적으로 설정 하면 모든 보기에 인증 정보를 추가할 필요가 없습니다. `setAadProperties` 

    맵 컨트롤에는 Android의 OpenGL 수명 주기를 관리 하는 자체 수명 주기 방법이 포함 되어 있습니다 .이를 포함 하는 작업에서 직접 호출 해야 합니다. 응용 프로그램이 올바르게 작동 하려면 맵 컨트롤의 수명 주기 메서드를 호출 하 고, 맵 컨트롤을 포함 하는 작업에서 다음 수명 주기 메서드를 재정의 하 고 해당 맵 컨트롤 메서드를 호출 해야 합니다. 

    * onCreate (번들) 
    * onStart () 
    * onResume () 
    * onPause () 
    * onStop () 
    * onDestroy () 
    * onSaveInstanceState (번들) 
    * onLowMemory () 

    다음과 같이 **Mainactivity. java** 파일을 편집 합니다.
    
    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);
    
            //Wait until the map resources are ready.
            mapControl.onReady(map -> {
                //Add your post map load code here.
    
            });
        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        protected void onStart(){
            super.onStart();
            mapControl.onStart();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }

    ```

## <a name="import-classes"></a>클래스 가져오기

위의 단계를 완료 한 후에는 일부 코드에 대 한 Android Studio에서 경고를 받게 됩니다. 이러한 경고를 해결 하려면에서 `MainActivity.java`참조 되는 클래스를 가져옵니다.

Alt + Enter (Mac에서 옵션 + 반환)를 선택 하 여 이러한 클래스를 자동으로 가져올 수 있습니다.

다음 그래픽에 표시 된 것 처럼 실행 단추를 선택 하거나 Mac에서 Control + R을 눌러 응용 프로그램을 빌드합니다.

![[실행]을 클릭합니다.](./media/how-to-use-android-map-control-library/run-app.png)

응용 프로그램을 빌드하는 데 몇 초 정도 걸릴 Android Studio. 빌드가 완료 되 면 에뮬레이트된 Android 장치에서 응용 프로그램을 테스트할 수 있습니다. 다음과 같은 지도가 표시 됩니다.

<center>

![Android 맵](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="localizing-the-map"></a>맵 지역화

Azure Maps Android SDK에서는 지도의 언어와 지역별 보기를 설정 하는 세 가지 다른 방법을 제공 합니다. 다음 코드에서는 언어를 프랑스어 ("fr-fr")로 설정 하 고 지역 보기를 "auto"로 설정 하는 방법을 보여 줍니다. 

첫 번째 옵션은 정적 `AzureMaps` `setLanguage` 및 `setView` 메서드를 전역적으로 사용 하 여 언어를 전달 하 고 지역 정보를 클래스에 표시 하는 것입니다. 그러면 앱에 로드 된 모든 Azure Maps 컨트롤에 대해 기본 언어 및 지역 보기가 설정 됩니다.

```Java
static {
    //Set your Azure Maps Key.
    AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

    //Set the language to be used by Azure Maps.
    AzureMaps.setLanguage("fr-FR");

    //Set the regional view to be used by Azure Maps.
    AzureMaps.setView("auto");
}
```

두 번째 옵션은 언어 및 뷰 정보를 맵 컨트롤 XML로 전달 하는 것입니다.

```XML
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

세 번째 옵션은 maps `setStyle` 메서드를 사용 하 여 맵의 언어 및 지역별 뷰를 프로그래밍 방식으로 설정 하는 것입니다. 언제 든 지이 작업을 수행 하 여 지도의 언어 및 지역별 보기를 변경할 수 있습니다.

```Java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

언어가 "fr-fr"로 설정 되 고 지역 보기가 "auto"로 설정 된 Azure Maps의 예는 다음과 같습니다.

<center>

![프랑스어로 레이블을 표시 하는 지도 이미지](./media/how-to-use-android-map-control-library/android-localization.png)
</center>

지원 되는 언어 및 지역 보기의 전체 목록은 [여기](supported-languages.md)에 설명 되어 있습니다.

## <a name="next-steps"></a>다음 단계

지도에 오버레이 데이터를 추가 하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [Android 맵에 기호 계층 추가](https://review.docs.microsoft.com/azure/azure-maps/how-to-add-symbol-to-android-map)

> [!div class="nextstepaction"]
> [Android 맵에 셰이프 추가](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Android maps에서 지도 스타일 변경](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)

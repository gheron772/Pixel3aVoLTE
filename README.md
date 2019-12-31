# Pixel 3a VoLTE patch
<hr/>

## 1. 개요
본 저장소는 Pixel 3a의 VoLTE patch를 위한 방법 및 원리를 설명한다.  
KT의 경우만 테스트 되었으며 SKT, U+에서 잘 동작하지 않을 경우  
mcfg_sw.mbn을 각 통신사 파일만 남기도록 수정하여 테스트가 필요하다.  
<br>

## 2. 사전준비
* OS version = sargo-qq1a.191205.011
* OMD registration (Like KT PTA-VOLTE)
* Bootloader unlock
* Rooting
* QPST & Qualcomm driver
* Open diag port patched magsik boot.img
<br>

## 3. 작업절차
3-1. 패치된 magisk boot.img를 Flashing 한다. 
[(magisk_patched.img)](https://github.com/gheron772/Pixel3aVoLTE/raw/master/files/magisk_patched.img)  
3-2. Pixel_2_Diag_Port.zip 모듈을 설치한다. 
[(Pixel_2_Diag_Port.zip)](https://github.com/gheron772/Pixel3aVoLTE/raw/master/files/Pixel_2_Diag_Port.zip)  
3-3. pixel3a volte 모듈을 설치한다. [(p3a_volte.zip)](https://github.com/gheron772/Pixel3aVoLTE/raw/master/files/p3a_volte.zip)  
3-4. EFS explorer를 실행 후 /google/user_agent_template 파일을 교체한다. 
[(user_agent_template)](https://github.com/gheron772/Pixel3aVoLTE/raw/master/files/user_agent_template)  
3-5. 재부팅 후 패치 적용 여부를 확인한다.  
3-6. Pixel_2_Diag_Port.zip 모듈을 제거한다.
<br>
<br>

## A. 부록
A-1. 사용된 mcfg_sw.mbn파일은 Razer phone2와 Poco phone의 파일이다.  
     Pixel 3a와 같은 sdm670 프로세서나 x12 모뎀이 아닌 경우에도 동작 하였다.  

A-2. Pixel 3a의 EFS 갱신 방법은 /data/vendor/modem_fdr을 제거 후 재부팅 하는 방식으로 Pixel 3와 같다.  

A-3. Pixel 3a modem firmware는 user_agent_template의 내용으로 부터 ims_user_agent를 생성한다.  
     user_agent_template의 내용은 Google_[MI__ap]_[SW__ap]으로 모델명, 빌드버전이 매핑된다.  
     ims_user_agent의 첫번째 데이터가 TTA-VoLTE/2.0이 아닌경우 VoLTE가 일부 또는 전부 정상적으로 동작하지 않는다.  

A-4. SKT Sim의 경우 user_agent_template를 패치하지 않아도 자동으로 TTA-VoLTE/2.0을 넣어줄 수 도 있다.(확인안됨)  

A-5. 본 문서의 내용으로 Pixel 3, Pixel 4, Pixel 4a, *XL도 패치가 될 것으로 예상된다.  
<br>
<br>

## B. 부록2
B-1. Magisk 소스에 https://github.com/AGagarin/Magisk/commit/0ffa5edd4b52cb002e360f70be075e9544dfcaf7 내용을  
Magisk-20.1\Magisk-20.1\native\jni\init\magiskrc.h 에 추가 후 빌드하면 diag기능이 활성화 된 magisk를 얻을 수 있다. 
[(build_magisk.zip)](https://github.com/gheron772/Pixel3aVoLTE/raw/master/files/build_magisk.zip)  

B-2. Magisk manager 소스에 아래 내용을
```kotlin
ZipInputStream(context.readUri(Uri.parse("file:///sdcard/Download/build_magisk.zip")).buffered()).use { zi ->
```
Magisk-20.1\Magisk-20.1\app\src\main\java\com\topjohnwu\magisk\tasks\MagiskInstaller.kt에 수정 후 빌드하면  
boot.img에 magisk를 덮어 쓸때 /sdcard/Download/build_magisk.zip 파일을 가져와 사용한다. 
[(app-debug.apk)](https://github.com/gheron772/Pixel3aVoLTE/raw/master/files/app-debug.apk)

B-3. 각 기종별 diag 드라이버는 AOSP 소스에서 /bonito/usb/UsbGadget.cpp의 내용을 확인하면 알 수 있다.(기종별 코드명)  
<br>
<br>

## Special thanks to
https://github.com/Magisk-Modules-Repo/volte-kr-crosshatch - nuriro  
https://cafe.naver.com/grnf/326870 - daedae
<br>
<br>

# 기부
본 문서가 도움이 되었다면  
<link href="https://fonts.googleapis.com/css?family=Lato&subset=latin,latin-ext" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/6EGDRwO"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy Coffee"><span style="margin-left:15px;font-size:19px !important;">Buy Coffee</span></a>

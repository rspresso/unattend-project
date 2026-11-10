# Windows Sysprep / Image Unattended Setup (unattend.xml)

Windows 11 Sysprep(시스템 준비 도구), WIM 이미지 배포 및 OOBE 무인 자동 설치 응답 파일(`unattend.xml`) 프로젝트입니다.

## 📌 autounattend.xml 과의 차이점 및 용도

| 구분 | `autounattend.xml` | `unattend.xml` |
| :--- | :--- | :--- |
| **적용 시점** | Windows 클린 설치 부팅 시 (Windows PE) | 이미지 배포/적용, 감사 모드 후 일반화(Generalize), Sysprep 후 첫 부팅 시 |
| **기본 배치 경로** | 부팅 USB 루트 (`X:\autounattend.xml`) | `C:\Windows\System32\Sysprep\unattend.xml` 또는 `C:\Windows\Panther\unattend.xml` |
| **주요 용도** | OS 설치 미디어를 통한 자동 클린 설치 | Sysprep 봉인 이미지 배포, OOBE 자동화 및 기존 시스템 프로비저닝 |

---

## 📋 주요 기능 및 포함 내역

1. **로컬 계정 자동 구성**
   - 사용자 계정명: `Midori` (관리자 권한 부여)
   - 첫 로그인 시 암호 설정 유도

2. **기본 시스템 최적화 & 한국어 로케일 설정**
   - 로케일: `ko-KR` (한국어 UI, 입력기 `0412:00000412`)
   - 표준 시간대: `대한민국 표준시` (Korea Standard Time)
   - 스냅 레이아웃 및 스냅 어시스트 비활성화
   - 원드라이브 정책 동기화 비활성화, 클라우드 소비자 기능 및 피드 비활성화 등

3. **모듈형 FirstLogon 자동화 스크립트 (`C:\Windows\Setup\Scripts\`)**
   - `Install-WinGet.ps1`: Microsoft WinGet 패키지 관리자 설치 및 최신화
   - `Install-Apps.ps1`: WinGet을 통한 필수 소프트웨어 자동 설치 (알파벳 정렬 적용)
     - `ImputNet.Helium`
     - `M2Team.NanaZip`
     - `MacType.MacType`
     - `Microsoft.Office` (Microsoft 365)
     - `RamenSoftware.Windhawk`
     - `Taiizor.SucroseWallpaperEngine`
     - `voidtools.Everything`
   - `Config-Notepad.ps1`: Windows 메모장 환경설정(`settings.dat`) 자동 배포
   - `Custom-Setup.cmd`: 사용자 정의 배치(CMD) 스크립트 실행 슬롯

---

## 🚀 사용 방법

### 방법 1. Sysprep 명령과 함께 실행
Sysprep으로 이미지를 봉인할 때 무인 응답 파일을 함께 전달합니다:
```cmd
%WINDIR%\System32\Sysprep\sysprep.exe /generalize /oobe /reboot /unattend:C:\Windows\System32\Sysprep\unattend.xml
```

### 방법 2. Panther 디렉터리에 사전 배치
Windows 배포 이미지나 설치 대상 드라이브의 Panther 폴더에 배치합니다:
```cmd
copy unattend.xml C:\Windows\Panther\unattend.xml
```

---

## ⚙️ 커스텀 배치(CMD) 추가 방법

`unattend.xml` 파일 하단의 `Custom-Setup.cmd` 태그 내 `<![CDATA[` 와 `]]>` 사이에 원하는 CMD 명령어들을 추가하시면 됩니다:

```xml
<File path="C:\Windows\Setup\Scripts\Custom-Setup.cmd"><![CDATA[@echo off
setlocal EnableExtensions

:: 여기에 실행할 명령어를 추가하세요.
echo [INFO] Running custom setup tasks...

endlocal
exit /b 0
]]></File>
```

---

## 📄 라이선스 (License)

이 프로젝트는 **GNU General Public License v3.0 (GPL-3.0)** 하에 배포됩니다.  
자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

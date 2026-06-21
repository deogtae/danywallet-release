# danywallet-release

DanyWallet 배포 바이너리(APK 등) **공개 다운로드** 저장소.

소스 코드는 별도 private 저장소(`danywallet.com`)에서 관리하고, 이 저장소는
공개 다운로드용 바이너리만 **GitHub Releases** 자산으로 배포합니다.

> GitHub은 저장소에 100MB 초과 파일 push를 거부하므로, APK는 git에 커밋하지 않고
> Releases 자산으로 업로드합니다. (`.gitignore`에서 `*.apk`, `*.zip` 제외)

## 핵심 규칙: 고정 파일명 + latest URL

홈페이지(`danywallet.com`)는 항상 **최신 릴리스**를 가리키는 고정 URL로 링크합니다:

```
https://github.com/deogtae/danywallet-release/releases/latest/download/<고정파일명>
```

| 다운로드 | 고정 파일명 |
| --- | --- |
| DanyWallet Android | `danywallet.apk` |
| 구 Cany Android | `cany.apk` |
| NativeScript Android | `todo-nativescript.apk` |
| Web Extension | `todo-webextension-solidjs.zip` |

`latest/download/<파일명>`은 "가장 최신 릴리스에서 그 이름의 자산"을 내려줍니다.
따라서 **파일명을 버전 없이 고정**해야 동작합니다. 버전 정보는 파일명이 아니라
**릴리스 태그/노트**에 둡니다. (예: `danywallet-1.2-b16.apk` ❌ → `danywallet.apk` ✅)

이 규칙 덕분에 **새 버전을 배포해도 홈페이지 링크는 고칠 필요가 없습니다.**

## 새 배포 파일이 생겼을 때 (업데이트 절차)

1. 새 빌드 파일을 **고정 파일명**으로 이 폴더(`release.git`)에 덮어씁니다.
   - 예: 새 빌드 `danywallet-1.3-b1.apk` → `danywallet.apk`로 이름 변경 후 이 폴더에 복사
   - (큰 파일은 `.gitignore`로 git에서 자동 제외됨 — 커밋되지 않습니다)

2. 새 태그로 릴리스를 만들고 **고정 파일명** 자산을 올립니다:

   ```bash
   cd release.git
   gh release create v1.3 \
     danywallet.apk cany.apk todo-nativescript.apk todo-webextension-solidjs.zip \
     -R deogtae/danywallet-release \
     --title "DanyWallet 다운로드 v1.3" \
     --notes "이번 버전 변경 사항..."
   ```

   - 가장 최근(비-prerelease) 릴리스가 자동으로 `latest`가 되므로,
     홈페이지의 `latest/download/...` 링크가 바로 새 파일을 가리킵니다.
   - `--prerelease` 플래그를 붙이면 `latest`에서 제외됩니다(테스트용).

3. (선택) 일부 파일만 새로 올릴 때는 새 태그 없이 기존 릴리스 자산을 교체:

   ```bash
   gh release upload v1.2 danywallet.apk -R deogtae/danywallet-release --clobber
   ```

## 검증

배포 후 익명(비로그인)으로 받아지는지 확인:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -L \
  https://github.com/deogtae/danywallet-release/releases/latest/download/danywallet.apk
# 200 이면 정상
```

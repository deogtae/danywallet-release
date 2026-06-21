# danywallet-release

DanyWallet 배포 바이너리(APK 등) **공개 다운로드** 저장소.

소스 코드는 별도 private 저장소(`danywallet.com`)에서 관리하고, 이 저장소는
공개 다운로드용 바이너리만 **GitHub Releases** 자산으로 배포합니다.

> GitHub은 저장소에 100MB 초과 파일 push를 거부하므로, APK는 git에 커밋하지 않고
> Releases 자산으로 업로드합니다. (`.gitignore`에서 `*.apk`, `*.zip` 제외)

## 핵심 규칙: 버전 포함 파일명 + 고정(pinned) URL

다운로드되는 파일명 = Release 자산의 실제 이름입니다. 사용자가 받은 파일에
버전이 보이도록 **파일명에 버전을 포함**합니다. (예: `danywallet-1.2-b16.apk`)

홈페이지(`danywallet.com`)는 각 파일을 **실제 릴리스 태그로 고정한 URL**로 링크합니다:

```
https://github.com/deogtae/danywallet-release/releases/download/<태그>/<버전포함파일명>
```

| 다운로드 | 현재 태그 | 파일명 | 웹 버전 표시 |
| --- | --- | --- | --- |
| DanyWallet Android | `v1.2` | `danywallet-1.2-b16.apk` | `1.2-b16` |
| 구 Cany Android | `v1.2` | `cany-1.1.apk` | `1.1` |
| NativeScript Android | `v1.2` | `todo-nativescript.apk` | — |
| Web Extension | `v1.2` | `todo-webextension-solidjs.zip` | — |

> 옛 릴리스는 삭제하지 않고 남겨둡니다. 그러면 일부 파일만 새 버전이 나와도
> 바뀌지 않은 파일의 고정 URL은 계속 동작합니다. (각 링크를 독립적으로 갱신 가능)

웹 페이지의 버전 표시는 홈페이지 `src/routes/index.tsx`의 각 항목 `version` 필드,
파일명 표시는 `linkText` 필드로 관리합니다.

## 새 배포 파일이 생겼을 때 (업데이트 절차)

예: DanyWallet을 `1.3-b1`로 새로 배포하는 경우

1. 새 빌드를 **버전 포함 파일명**으로 이 폴더(`release.git`)에 둡니다.
   - 예: `danywallet-1.3-b1.apk`
   - (큰 파일은 `.gitignore`로 git에서 자동 제외됨 — 커밋되지 않습니다)

2. 새 태그로 릴리스를 만들고 그 파일을 올립니다:

   ```bash
   cd release.git
   gh release create v1.3 danywallet-1.3-b1.apk \
     -R deogtae/danywallet-release \
     --title "DanyWallet 1.3-b1" \
     --notes "이번 버전 변경 사항..."
   ```

3. 홈페이지 `danywallet.com`의 `src/routes/index.tsx`에서 **해당 항목만** 수정:

   ```ts
   {
     project: "DanyWallet Android (Preview)",
     version: "1.3-b1",                                   // 웹에 보이는 버전
     url: "https://github.com/deogtae/danywallet-release/releases/download/v1.3/danywallet-1.3-b1.apk",
     linkText: "danywallet-1.3-b1.apk",                   // 링크에 보이는 파일명
   },
   ```

   바뀌지 않은 다른 항목(예: Cany)은 그대로 두면 됩니다 — 옛 `v1.2` 릴리스가
   남아 있어 기존 링크가 계속 동작합니다.

4. (대안) 버전/파일명을 그대로 두고 내용만 교체할 때는 기존 자산 덮어쓰기:

   ```bash
   gh release upload v1.2 danywallet-1.2-b16.apk -R deogtae/danywallet-release --clobber
   ```

   이 경우 홈페이지 수정은 불필요합니다.

## 검증

배포 후 익명(비로그인)으로 받아지는지 확인:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -L \
  https://github.com/deogtae/danywallet-release/releases/download/v1.2/danywallet-1.2-b16.apk
# 200 이면 정상
```

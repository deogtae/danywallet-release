# danywallet-release

DanyWallet 배포 바이너리(APK 등) 공개 다운로드 저장소.

소스 코드는 별도의 private 저장소(`danywallet.com`)에서 관리하고,
이 저장소는 **공개 다운로드용 바이너리만** GitHub Releases로 배포합니다.

> GitHub은 저장소에 100MB 초과 파일 push를 거부하므로, APK는 git에 커밋하지 않고
> Releases 자산으로 업로드합니다. (`.gitignore`에서 `*.apk`, `*.zip` 제외)

## 다운로드

최신 파일은 [Releases](https://github.com/deogtae/danywallet-release/releases) 에서 받을 수 있습니다.

## 새 버전 배포 방법

```bash
# 로컬 release.git 폴더에 새 파일을 둔 뒤
gh release create <태그> *.apk *.zip \
  -R deogtae/danywallet-release \
  --title "<제목>" --notes "<설명>"

# 기존 릴리스에 파일만 추가/교체할 때
gh release upload <태그> <파일> -R deogtae/danywallet-release --clobber
```

다운로드 URL 형식:
`https://github.com/deogtae/danywallet-release/releases/download/<태그>/<파일명>`

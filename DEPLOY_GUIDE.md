# SUPPORTY 홈페이지 — 데스크톱 배포 가이드 (그대로 따라하기)

사이트는 완성되어 있습니다 (index.html + demo.html + assets/).
아래 순서대로만 하면 심사위원이 클릭할 수 있는 공개 URL이 나옵니다.

---

## STEP 0. 파일 준비 (5분)

1. 다운로드 받은 `supporty-site` 폴더를 바탕화면에 둡니다
2. `assets/README.md`에 적힌 파일명대로 영상·사진을 assets 폴더에 넣습니다:
   - `demo_live.mp4` ← 라이브 데모 화면녹화
   - `trailer.mp4` ← 트레일러 완성본
   - `op_ugv.mp4`, `op_vtol.mp4` ← Veo 클립 (파일명 변경)
   - `hw_ugv.jpg`, `hw_vtol.jpg` ← 실물 사진
3. `index.html`을 메모장/VSCode로 열어 `YOUR_REPO`를 실제 GitHub 주소로 바꿉니다 (2군데)

> 영상이 아직 없어도 배포 가능합니다 — 빈 슬롯에 파일명 안내가 뜨고, 나중에 파일만 추가 커밋하면 됩니다.

---

## STEP 1-A. GitHub Pages 배포 (추천 — 이미 GitHub 쓰고 계시니)

VSCode 터미널(또는 PowerShell)에서, supporty-site 폴더 기준:

```bash
cd C:\Users\SAMSUNG\Desktop\supporty-site

git init
git add .
git commit -m "Supporty official site"

# GitHub에서 새 레포 만들기 (브라우저: github.com/new → 이름: supporty-site → Public → Create)
# 만든 뒤 아래 두 줄 (YOUR_ID를 본인 아이디로):
git remote add origin https://github.com/YOUR_ID/supporty-site.git
git branch -M main
git push -u origin main
```

푸시 후 브라우저에서:
1. 레포 페이지 → **Settings** → 왼쪽 메뉴 **Pages**
2. Source: **Deploy from a branch** → Branch: **main** / **(root)** → Save
3. 1~2분 후 상단에 URL 표시됨: `https://YOUR_ID.github.io/supporty-site/`

이 URL이 제출용 데모 URL입니다. `…/demo.html`은 콘솔 데모 직링크.

### 영상 파일이 큰 경우 (50MB 넘는 파일이 있으면)
GitHub는 파일당 100MB 제한, 50MB부터 경고가 뜹니다. mp4가 크면:
```bash
# 옵션 1: 압축 (HandBrake 설치되어 있으면 GUI로, 또는 ffmpeg)
ffmpeg -i trailer.mp4 -vf scale=1280:720 -crf 26 trailer_web.mp4
# → 파일명 맞춰서 교체

# 옵션 2: 영상만 YouTube Unlisted로 올리고, index.html의 <video> 블록을
#          유튜브 iframe으로 교체 (아래 STEP 3 참고)
```

---

## STEP 1-B. 더 빠른 대안 — Netlify Drop (2분, 계정 불필요)

Git이 꼬이거나 시간이 없으면:
1. https://app.netlify.com/drop 접속
2. `supporty-site` 폴더를 통째로 브라우저 창에 드래그
3. 즉시 `https://무작위이름.netlify.app` URL 발급 → 그대로 제출

단점: 나중에 파일 수정하려면 다시 드래그해야 함 (GitHub Pages는 push만 하면 됨)

---

## STEP 2. Claude Code(CLI)에게 시킬 경우의 프롬프트

직접 명령어 치는 대신 Claude Code한테 맡기려면, VSCode에서 supporty-site 폴더 열고:

```
이 폴더는 완성된 정적 사이트야(index.html, demo.html, assets/).
GitHub Pages로 배포해줘:
1. git 초기화하고 전체 커밋
2. gh CLI가 설치되어 있으면 `gh repo create supporty-site --public --source=. --push`로
   레포 생성+푸시까지, 없으면 수동 remote 추가 방법 안내
3. gh CLI로 Pages 활성화: `gh api repos/{owner}/supporty-site/pages -X POST -f build_type=legacy -f "source[branch]=main" -f "source[path]=/"`
4. 최종 공개 URL 알려줘
assets 폴더 안에 50MB 넘는 mp4가 있으면 푸시 전에 알려주고 ffmpeg 압축 명령을 제안해줘.
```

---

## STEP 3. (선택) 영상을 YouTube로 대체하는 방법

용량 문제가 있거나 재생 안정성이 걱정되면, 해당 영상의 `<div class="video-frame">…</div>` 블록 내부를 이걸로 교체:

```html
<iframe style="width:100%; aspect-ratio:16/9; border:0;"
  src="https://www.youtube.com/embed/영상ID"
  allowfullscreen></iframe>
```

---

## STEP 4. 제출 직전 확인

- [ ] 공개 URL이 시크릿창에서도 열리는지 (로그인 없이)
- [ ] 모바일에서도 열어보기 (심사위원이 폰으로 볼 수 있음)
- [ ] demo.html 링크 정상 동작
- [ ] index.html 안의 GitHub 링크(YOUR_REPO) 실제 주소로 교체됐는지
- [ ] 제출 폼의 "데모 URL" 칸에 입력

# macOS 설치 명령 모음 — CogFlow 비서 (Day 1)

macOS는 보안(Gatekeeper) 때문에 터미널 명령이 몇 개 필요합니다. 타이핑하지 말고
각 명령 블록 오른쪽 위의 **복사 버튼**을 눌러 **터미널에 붙여넣기(⌘V) → Enter**로 실행하세요.
명령은 **실제 작업 순서 그대로**이니 **위에서부터 차례대로** 진행합니다.
(Windows 수강생은 이 문서가 필요 없습니다 — bat 더블클릭으로 진행)

> 💡 **터미널 여는 법** — **⌘Space**(Spotlight) → **"터미널"** 입력 → Enter.
> 이후 모든 명령은 이 터미널 창 하나에서 순서대로 실행합니다.

---

## 1단계 — 배포 zip 풀기 *(zip을 받은 위치에서 시작)*

### ① zip을 받은 폴더로 이동

```sh
cd ~/Downloads
```

> zip을 다운로드 폴더가 아닌 곳(USB·바탕화면 등)에 뒀으면: 터미널에 `cd `(cd와 공백)만 입력하고
> **Finder에서 zip이 있는 폴더를 터미널 창으로 드래그** → Enter.

### ② 암호 zip 압축 해제 ★

```sh
mkdir -p ~/Day1 && tar -xf CogFlow_Day1.zip -C ~/Day1
```

> 반드시 이 **tar** 명령으로 푸세요 — 더블클릭(기본 압축 유틸리티)은 암호 zip을 못 열고,
> **unzip 명령은 한글 파일명에서 "Illegal byte sequence" 오류로 중단**됩니다(macOS unzip 결함).
> 실행하면 **Enter passphrase:** 가 나옵니다 — **진행자가 안내한 비밀번호**를 입력하세요
> (입력해도 화면에 표시되지 않는 것이 정상입니다. 프롬프트가 안 뜨면
> `tar -xf CogFlow_Day1.zip --passphrase '비밀번호' -C ~/Day1` 처럼 직접 지정).
> 홈 폴더 아래 **Day1** 폴더에 **kbpn**·**실습팩**·**skills** 세 폴더가 풀립니다.

### ③ kbpn 폴더로 이동

```sh
cd ~/Day1/kbpn
```

> 이후 명령은 전부 **kbpn 폴더 안**이라는 전제로 실행합니다.

---

## 2단계 — Node.js 확인 *(22.5 이상 필요)*

### ④ Node.js 버전 확인

```sh
node --version
```

> v22.5.0 이상이면 통과 → ⑥으로. "command not found"이거나 버전이 낮으면 ⑤로 설치하세요.

### ⑤ (필요 시) Node.js 설치 — Homebrew

```sh
brew install node@22 && brew link --overwrite node@22
```

> Homebrew가 없으면 nodejs.org에서 macOS 공식 pkg(22 LTS)를 내려받아 설치해도 됩니다. 설치 후 ④로 다시 확인.

---

## 3단계 — 권한 준비 *(최초 1회 · 가장 중요한 단계)*

### ⑥ Gatekeeper 격리 해제 + 실행 권한 복원 ★

```sh
xattr -dr com.apple.quarantine . && chmod +x *.command
```

> 이 단계를 건너뛰면 .command 더블클릭 시 **"확인되지 않은 개발자"** 로 차단됩니다(최대 함정).
> xattr = 격리 속성 제거, chmod = Windows에서 만든 zip이 잃어버린 실행 권한 복원.
> ③에서 kbpn 폴더로 이동한 상태여야 합니다.

---

## 4단계 — 설치·기동 *(Windows의 bat 더블클릭에 해당)*

### ⑦ (선택) cogflow 명령 전역 등록

```sh
./setup-kbpn.command
```

> 체험판 배포에서는 건너뛰어도 비서가 동작합니다. 오후 실습(M6·M7)에서 아무 폴더에서나
> cogflow 명령을 쓰려면 실행해 두세요.

### ⑧ 내 비서 만들기 — 봇 토큰·chat id 입력

```sh
./setup-kbsa.command
```

> 실행 전에 준비: Telegram에서 **@BotFather → /newbot → 봇 토큰 복사**(봇 아이디는 bot으로 끝나야 함) →
> 내 봇에게 아무 메시지 1회 전송 → **@userinfobot**으로 내 숫자 chat id 확인.
> 마법사가 물으면 토큰 → chat id 순서로 붙여넣습니다.

### ⑨ 비서 기동 — 터미널 창 2개

```sh
./start-kbsa.command
```

> Terminal 창 2개(비서 서버 → 게이트웨이)가 순서대로 뜹니다. 첫 응답까지 수십 초 걸리는 것이 정상.
> 끝나면 Telegram에서 내 봇에게 "오늘 회의비 5만원 썼어"라고 말을 걸어 보세요.

---

## 종료 *(교육 끝·재부팅 시)*

### ⑩ 종료

```sh
./stop-kbsa.command
```

> 기록·지식은 `~/.cogflow/` 폴더에 남습니다 — 번들을 지워도 비서의 기억은 보존되고,
> 노트북을 가져가면 비서도 함께 갑니다(1인 1비서).

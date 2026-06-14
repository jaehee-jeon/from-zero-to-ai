# from-zero-to-ai
Learn by teaching — Python, Azure, and AI study group
# 🧠 Learn by Teaching
### Python · Azure · AI - Sesac MS AI Engineer 수업 복습 스터디

> 아는 것은 설명할 수 있을 만큼, 모르는 것은 "이제 나도 안다" 할 수 있을 만큼.

---

## 🤖 이 repo는 이렇게 굴러갑니다

스터디 운영의 반복 작업을 **GitHub Actions + Claude API + Teams Webhook**으로 자동화했습니다.
스터디장이 없어도 굴러가는 시스템이 목표입니다.

```
GitHub Actions (cron + push 트리거)
   ├─ 발표자 사다리타기 자동 배정
   ├─ Claude API → 발표 가이드 질문 자동 생성
   ├─ Claude API → 실습 문제지 자동 생성 → quiz/ 폴더에 push
   └─ Teams Webhook 알림
        ├─ #study-weekly : 발표자 배정 · 업로드 인증
        └─ #study-board  : 1인 1발표 티켓 · 가이드 질문
```

- 발표자마다 **본인 이름이 붙은 티켓**이 자동 발급
- 풀이를 본인 폴더에 올리면 GitHub 기록으로
- 단, 주제 수집(`config/topics.json`)은 Teams 댓글을 읽는 API가 없어 아직 수동😅

워크플로 코드는 [`.github/workflows`](.github/workflows)에 있습니다.

---


## 🧩 스터디 진행 방식

1. **매주 금요일 오전** — 스터디장이 주제 2개 수집 (어려웠던 것 1 + 중요한 것 1)
2. **금요일 오후 4시** — 투표로 10개 주제 선정
3. **사다리타기** — 발표자 배정 (각 주제당 1명)
4. **모임 전날까지** — 발표 자료 준비 + GitHub 업로드 (10분 이내)
5. **모임 날** — 발표 ✅


---

## 👥 스터디 멤버

| 이름 | GitHub | Path | 비고 |
|------|--------|--------|--------|
| 전재희 | [@jaehee-jeon](https://github.com/jaehee-jeon) | [jaeheejeon](./jaeheejeon/) |*|
| 김제니 | [@Kim-Eunhyang](https://github.com/Kim-Eunhyang) | [jenny](./jenny/) ||
| 김석영 | [@seokyeong-kim](https://github.com/seokyeong-kim) | [seokyeong-kim](./seokyeong-kim/) ||
| 김예찬 | [@citizen-ye](https://github.com/citizen-ye) | [kimyechan](./kimyechan/) ||
| 박성은 | [@TAEstationSUNG](https://github.com/TAEstationSUNG) | [sungeun](./sungeun/) ||
| 이준석 | [@Leejs-js](https://github.com/Leejs-js) | [leejunseok](./leejunseok/) |
| 임치영 | [@yim1jp](https://github.com/yim1jp) | [yim](./yim/) ||
| 황선주 | [@seonju](https://github.com/seonju) | [seonju](./seonju/) ||
| 정승영 | [@sampython1](https://github.com/sampython1) | [Sam](./Sam/) ||
| 최진웅 | [@bareph](https://github.com/bareph) | [JinWoong](./JinWoong/) ||



---

## 📅 주차별 진행 계획

| 주차 | 주제 | 날짜 | 상태 |
|------|------|------|------|
| 1주차 | git set up 및 workflow 설명 | 6/2| 완료 |
| 2주차 | Python 기본문법 + 웹데이터 수집 | 6/10 | 완료 |
| 3주차 | 🚫 프로젝트 기간 (휴식) | 6/17 ~ 6/25 | — |
| 4주차 | 데이터 정제 + AzureOpenAI 솔루션 | 6/29 ~ 7/3 | 🔜 예정 |
| n주차 | 연장 예정... |  | |

---

## 🚀 처음 시작하는 법

```bash
# 1. 레포 클론
git clone https://github.com/YOUR_USERNAME/from-zero-to-ai.git

# 2. 폴더 진입
cd from-zero-to-ai

# 3. 내 파일 작성 후 업로드
git add .
git commit -m "Docs: 1주차 정리 - 홍길동"
git push
```


---

## 🔗 참고 링크

- [스터디 노션 모집글][(https://sweltering-television-f8a.notion.site/36cb48b0587c8092ae94d22a474d8727)](https://sweltering-television-f8a.notion.site/36cb48b0587c8092ae94d22a474d8727?source=copy_link)
- [git 기본 명령어 정리 (OT 자료)] — [https://www.notion.so/06-02-git-373b48b0587c8091a1f0e30a35c69e57?source=copy_link](https://sweltering-television-f8a.notion.site/06-02-git-373b48b0587c8091a1f0e30a35c69e57?source=copy_link)
  

---

<p align="center">
  <i>같이하면 달라집니다. 일단 4번만 해봐요 🥹</i>
</p>

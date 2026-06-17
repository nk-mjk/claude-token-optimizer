![token-optimizer](./token-optimizer-title.png)

# token-optimizer

> **AI도 아껴 써야 한다.**  
> LLM이 하던 일 중 Python이 대신할 수 있는 것을 찾아내고, 승인 후 실제로 바꿔준다.

---

## 왜 만들었나

스킬을 만들다 보면 LLM에게 굳이 시킬 필요 없는 일들이 섞여 들어간다.  
파일이 있는지 확인하고, 파일 수를 세고, 정해진 패턴으로 텍스트를 치환하고—  
이런 건 Python 한 줄이면 충분한데 매 실행마다 토큰을 쓴다.

token-optimizer는 기존 스킬의 SKILL.md를 분석해 **Python으로 확실히 대체 가능한 부분**만 골라내고, 사용자 승인 후 실제 스크립트로 바꿔준다.

---

## 핵심 원칙

**보수적으로 잡는다.**  
100% 확실한 것만 수정한다. 경계선 케이스는 리포트에만 적고 절대 손대지 않는다.  
스킬이 망가지면 신뢰도가 무너지기 때문이다.

---

## Python 대체 확정 4종

| 유형 | 판정 기준 |
|------|-----------|
| **파일 I/O** | 입출력이 둘 다 파일. 형식 이해 없이 읽고 결합·분리 |
| **카운팅** | 숫자만 세는 작업. 판단 없음 |
| **텍스트 치환** | 고정 패턴을 다른 문자열로 일괄 교체 |
| **파일 존재 확인** | True/False만 반환. 판단 없음 |

위 4종에 해당하지 않으면 건드리지 않는다. 창작·자연어 해석·품질 판단은 LLM 영역이다.

---

## 동작 흐름

```
Phase 1: 스킬 분석
  → 전체 단계를 읽고 확정 / 경계선 / LLM 필수 3가지로 분류
  → 리포트 출력

✅ Approval Gate 1 — 사용자 확인 후 진행

Phase 2: Python 스크립트 작성
  → 확정 대상만 스크립트로 작성, 대상 스킬 scripts/ 폴더에 저장
  → 작성된 스크립트 전체 화면 표시

✅ Approval Gate 2 — 사용자 확인 후 진행

Phase 3: SKILL_optimized.md 저장
  → 원본 SKILL.md는 절대 건드리지 않음
  → 수정본을 SKILL_optimized.md로 별도 저장
```

---

## 사용 방법

다음 표현 중 하나로 실행하면 자동 트리거된다:

- `이 스킬 토큰 최적화해줘`
- `token-optimizer 시작`
- `SKILL.md 분석해서 Python 대체 찾아줘`
- `스킬 런타임 줄여줘`
- `파이썬 스크립트로 바꿔줘`

---

## 설치

1. `token-optimizer.skill` 파일 다운로드
2. Claude Cowork → Settings → Capabilities → Install Skill

---

## 검증 결과 (iteration-1)

| 구분 | 통과율 |
|------|--------|
| with skill | **93.3%** |
| without skill | 43.3% |
| Delta | **+50%p** |

테스트 스킬: webtoon-2 · aitoon-insta · aitoon-plot

---

## 제작자

[@bluemisty](https://github.com/bluemisty) — Claude Cowork로 제작

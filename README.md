# only-catalog-for-demo

`marketplace.json` **"참조만(reference-only)"** 방식 데모.

이 레포에는 **`marketplace.json` 한 파일만** 있습니다. 외부 plugin 코드를 사내 repo에 복사하지 않고, 카탈로그(매니페스트)로만 노출해서 사용자가 `/plugin install`을 실행할 때 외부 repo에서 직접 clone되도록 하는 패턴을 보여줍니다.

## 구조

```
only-catalog-for-demo/
└── .claude-plugin/
    └── marketplace.json   ← 카탈로그 매니페스트 (전부)
```

## 노출하는 plugin

| plugin | 원본 |
|---|---|
| `handoff-skill` | [ykdojo/claude-code-tips](https://github.com/ykdojo/claude-code-tips) |
| `context-engineering` | [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering) |
| `rtk` | [rtk-ai/rtk](https://github.com/rtk-ai/rtk) |

모두 외부 repo를 **그대로 참조**하며, 이 레포에는 코드가 복사되어 있지 않습니다.

## 사용 방법 (Claude Code CLI)

### 1) 마켓플레이스 등록

```bash
/plugin marketplace add jungtaekkim123/only-catalog-for-demo
```

### 2) 카탈로그 확인

```bash
/plugin
```

### 3) 설치

```bash
/plugin install handoff-skill@only-catalog-demo
/plugin install context-engineering@only-catalog-demo
/plugin install rtk@only-catalog-demo
```

### 4) 업데이트 / 제거

```bash
# 외부 repo의 변경 사항을 카탈로그에 반영
/plugin marketplace update only-catalog-demo

# 마켓플레이스 제거 (해당 plugin도 함께 제거됨)
/plugin marketplace remove only-catalog-demo
```

## 검증

배포 전 매니페스트 구문 확인:

```bash
claude plugin validate .
```

> 주의: 이 검증은 `marketplace.json`의 **구문/스키마**만 확인합니다. 외부 repo가 살아있는지 같은 **런타임 결합 문제는 `/plugin install` 시점**에 비로소 드러납니다.

## 참조만 방식의 트레이드오프

| 장점 | 단점 |
|---|---|
| 사내 repo가 가벼움 (매니페스트 1개 파일) | 외부 repo에 의존 — 오프라인 시 설치 불가 |
| 외부 변경이 자동 반영됨 | 외부 변경이 자동 반영됨 (양날의 검) |

외부 변경 위험을 줄이려면 `source`에 `ref`(브랜치/태그) 또는 `sha`(정확한 커밋)로 핀(pin)을 박는 것이 안전합니다.

## 관련 문서

- [Anthropic - 플러그인 마켓플레이스](https://code.claude.com/docs/ko/plugin-marketplaces)
- [사내 운영안 - Skill Marketplace](https://ignitecorp.atlassian.net/wiki/spaces/AID/pages/2386362388/Skill+-+Marketplace)

# buwoo-status — 부우 서버 외부 감시

알림: 텔레그램 @buwoo_alert_bot (대표 직통)

## 현행 구조 (2026-06-13 확정)

| 감시 | 어디서 | 주기 | 상태 |
|---|---|---|---|
| 장애 감지 (6도메인) | **Cloudflare Worker `buwoo-watch`** (메인 repo `deploy/cf-worker-watch/`) | 1분 | ✅ 운영 중 |
| 아침 브리핑 (정상 보고 + 백업 나이) | **Cloudflare Worker `buwoo-watch`** | 매일 09:00 KST 정각 | ✅ 운영 중 |
| 백업 dead-man (R2 덤프 26h 초과 경보) | 이 repo `backup-deadman.yml` | 매일 04:47 KST | ✅ active |
| ~~uptime-probe.yml~~ | 이 repo | — | ⚠️ **disabled — 재활성화 금지** (Worker 로 대체, 켜면 이중 알림) |
| ~~morning-briefing.yml~~ | 이 repo | — | ⚠️ **disabled — 재활성화 금지** (Worker 로 대체) |

- 이 repo 가 공개인 이유: 메인 계정의 private repo Actions 가 결제 차단 상태 (공개 repo 는 무료·무제한)
- GitHub cron 은 실측 1.5~3.5h 지연 → 분 단위 감시·정각 발송은 전부 Worker 담당
- backup-deadman 은 매월 1일 keepalive 빈 커밋으로 60일 무활동 자동비활성을 방지

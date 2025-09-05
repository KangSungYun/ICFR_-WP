# 대손충당금 업무 흐름도 (Mermaid)

```mermaid
flowchart LR

  %% ─── Sales / AR ───
  start([Start · 월말 마감 트리거]) --> ar_close[AR 보조원장 마감 및 판매반품/조정 반영]

  %% ─── Credit Risk / FP&A ───
  ar_close --> aging[AR 에이징 리포트 추출·검토]
  aging --> segment[[세그먼트 구분 (고객군/지역/리스크)]]

  segment --> params[/ECL 파라미터 업데이트 (PD·LGD, 매크로)/]

  %% ─── Accounting ───
  params --> calc["ECL 계산 실행 (무상각·평균손실율)"]
  calc --> variance{전기 대비 변동 / 한도 초과?}
  variance -- 예 --> overlay[원인 분석 및 관리자 오버레이]
  variance -- 아니오 --> je_draft[분개전표 초안 작성 (충당금 설정·환입)]
  overlay --> je_draft --> approval{승인 권한 충족? (한도 기준)} --> post[전표 승인 및 GL 반영] --> recon([보조원장–총계정 대사 완료])

  %% ─── Internal Audit / Compliance ───
  recon --> disclosure[주석·공시자료 업데이트]
  disclosure --> evidence[증빙 보관 및 통제 수행 증적]
  evidence --> monitor[(모니터링/샘플 테스트 (월/분기))]
  monitor --> end([End · 마감 완료])

  %% ─── Key Controls (주석 노드) ───
  K1[[K1: 원장–리포트 일치 검증]]:::note --- aging
  K2[[K2: 파라미터 변경 승인]]:::note --- params
  K3[[K3: 변동성 한도 모니터링]]:::note --- variance
  K4[[K4: 관리자 오버레이 근거]]:::note --- overlay
  K5[[K5: 권한기반 승인]]:::note --- approval
  K6[[K6: 전표 번호/타임스탬프 로그]]:::note --- post
  K7[[K7: GL 대사 검토]]:::note --- recon
  K8[[K8: 증적 보관 및 접근통제]]:::note --- evidence

  classDef note fill:#fff8dc,stroke:#c8aa6e,color:#333,stroke-width:1px;
```

# 대손충당금 업무 흐름도 (Mermaid)

```mermaid
flowchart LR
  subgraph L1[Sales / AR]
    start([Start · 월말 마감]) --> ar_close[AR 보조원장 마감<br/>및 판매반품/조정 반영]
  end

  subgraph L2[Credit Risk / FP&A]
    aging[AR 에이징 리포트<br/>추출·검토] --> segment[[세그먼트 구분<br/>(고객군/지역/리스크)]] --> params[/ECL 파라미터 업데이트<br/>(PD·LGD, 매크로)/]
  end
  subgraph L3[Accounting]
    calc[\"ECL 계산 실행<br/>(무상각·평균손실율)\"] --> variance{전기 대비 변동 /<br/>한도 초과?}
    variance -- 예 --> overlay[원인 분석 및<br/>관리자 오버레이]
    variance -- 아니오 --> je_draft[분개전표 초안 작성<br/>(충당금 설정·환입)]
    overlay --> je_draft --> approval{승인 권한 충족?<br/>(한도 기준)} --> post[전표 승인 및 GL 반영] --> recon([보조원장–총계정<br/>대사 완료])
  end

  subgraph L4[Internal Audit / Compliance]
    disclosure[주석·공시자료 업데이트] --> evidence[증빙 보관 및 통제<br/>수행 증적] --> monitor[(모니터링/샘플 테스트<br/>(월/분기))] --> end([End · 마감 완료])
  end

  ar_close --> aging
  params --> calc
  recon --> disclosure


```mermaid
%%{init: {'flowchart': {'htmlLabels': false}} }%%
graph LR
  start([Start - Month End Trigger])-->ar_close[AR Subledger Close and Returns/Adjustments]
  ar_close-->aging[AR Aging Report Review]
  aging-->segment[Segmenting (Customer/Region/Risk)]
  segment-->params[Update ECL Parameters (PD, LGD, Macro)]
```

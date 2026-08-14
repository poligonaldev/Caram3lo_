# Design Técnico: Motor de Orquestração (05-scheduler)

## 🏗️ Fluxo de Sequência da Orquestração (`#design-orquestracao`)

```mermaid
sequenceDiagram
    autonumber
    participant SCH as Scheduler (Cron)
    participant SCR as Farejando (Scrapers)
    participant VAL as Rabo Levantado (Validator)
    participant STO as Pote de Ração (Storage)
    participant NOT as Latidos (Notifiers)

    SCH->>SCR: runAllScrapers()
    SCR-->>SCH: Array~RawEdital~
    loop Para cada RawEdital
        SCH->>VAL: validate(rawEdital)
        VAL-->>SCH: ValidatedEdital
        SCH->>STO: save(validatedEdital)
        alt Se for NOVO ou PRORROGADO e não notificado
            SCH->>NOT: dispatch(validatedEdital)
            NOT-->>STO: logNotification()
        end
    end
    SCH->>SCH: logExecutionSummary()
```

---

## 💻 Ponto de Entrada CLI (`src/index.ts`)

```typescript
import { Scheduler } from './scheduler/runner.js';

const scheduler = new Scheduler();

if (process.env.RUN_ONCE === 'true') {
  await scheduler.executeCycle();
} else {
  scheduler.startCronJob('0 * * * *'); // A cada hora
}
```

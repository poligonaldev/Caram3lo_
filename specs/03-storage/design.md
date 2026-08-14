# Design Técnico: Motor Pote de Ração (03-storage)

## 🏗️ Modelo Entidade-Relacionamento (`#design-pote-de-racao`)

```mermaid
erDiagram
    SOURCES ||--o{ EDITAIS : monitora
    EDITAIS ||--o{ EDITAL_VERSIONS : possui
    EDITAIS ||--o{ NOTIFICATION_LOGS : gera

    SOURCES {
        string id PK
        string name
        string baseUrl
        string status
        datetime lastScrapedAt
    }

    EDITAIS {
        string id PK
        string sourceId FK
        string contentHash
        string title
        string pageUrl
        datetime endDate
        string status
        datetime createdAt
        datetime updatedAt
    }

    EDITAL_VERSIONS {
        string id PK
        string editalId FK
        datetime oldEndDate
        datetime newEndDate
        datetime recordedAt
    }

    NOTIFICATION_LOGS {
        string id PK
        string editalId FK
        string channel
        string status
        datetime sentAt
    }
```

---

## 💻 Interface do Repositório (`EditalRepository`)

```typescript
export interface IEditalRepository {
  findByContentHash(hash: string): Promise<Edital | null>;
  findByPageUrl(url: string): Promise<Edital | null>;
  save(edital: ValidatedEdital): Promise<Edital>;
  markAsProrrogated(id: string, newEndDate: Date): Promise<void>;
  logNotification(editalId: string, channel: 'DISCORD' | 'WHATSAPP' | 'WEBSITE'): Promise<void>;
  wasNotified(editalId: string, channel: string): Promise<boolean>;
}
```

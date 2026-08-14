# Design Técnico: Motor Latidos (04-notifications)

## 🏗️ Arquitetura do Componente (`#design-latidos`)

```mermaid
classDiagram
    class INotifier {
        <<interface>>
        +name: string
        +send(edital: ValidatedEdital): Promise~boolean~
    }

    class DiscordNotifier {
        +send(edital: ValidatedEdital): Promise~boolean~
        #buildEmbed(edital: ValidatedEdital): DiscordEmbed
    }

    class WhatsAppNotifier {
        +send(edital: ValidatedEdital): Promise~boolean~
        #buildTextMessage(edital: ValidatedEdital): string
    }

    class WebFeedNotifier {
        +send(edital: ValidatedEdital): Promise~boolean~
        #updateFeedJson(): Promise~void~
    }

    class NotificationDispatcher {
        +notifiers: INotifier[]
        +dispatch(edital: ValidatedEdital): Promise~void~
    }

    INotifier <|.. DiscordNotifier
    INotifier <|.. WhatsAppNotifier
    INotifier <|.. WebFeedNotifier
    NotificationDispatcher o-- INotifier
```

---

## 🎨 Exemplo de Payload do Discord Embed

```json
{
  "embeds": [
    {
      "title": "🚨 NOVO EDITAL: Prêmio Funjope de Cultura Pop",
      "description": "Incentivo financeiro a projetos artísticos de João Pessoa.",
      "color": 5814783,
      "fields": [
        { "name": "🏛️ Órgão", "value": "Funjope - João Pessoa", "inline": true },
        { "name": "📅 Inscrições até", "value": "28/08/2026", "inline": true },
        { "name": "👤 Elegibilidade", "value": "Pessoa Física (PF) e Jurídica (PJ)", "inline": false }
      ],
      "url": "https://joaopessoa.pb.gov.br/secretarias/funjope/edital-1"
    }
  ]
}
```

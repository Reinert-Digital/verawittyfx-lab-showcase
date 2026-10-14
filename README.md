<p align="center">
  <img src="assets/banner.svg" alt="VeraWittyFX Lab" width="100%">
</p>

<p align="center">
  <b>Full-Stack Trading-Education-Plattform</b> mit Kursen, täglichen Marktanalysen, Live-Calls,<br>
  Community, Trading-Journal mit MetaTrader-Sync, Abo- und Ratenzahlungs-Billing und nativer Mobile-App.
</p>

<p align="center">
  <img alt="Next.js" src="https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-15-4169E1?logo=postgresql&logoColor=white">
  <img alt="Prisma" src="https://img.shields.io/badge/Prisma-5-2D3748?logo=prisma&logoColor=white">
  <img alt="Stripe" src="https://img.shields.io/badge/Stripe-Billing-635BFF?logo=stripe&logoColor=white">
  <img alt="Tailwind" src="https://img.shields.io/badge/Tailwind-3-06B6D4?logo=tailwindcss&logoColor=white">
  <img alt="Capacitor" src="https://img.shields.io/badge/Capacitor-iOS%20%7C%20Android-119EFF?logo=capacitor&logoColor=white">
  <img alt="Tests" src="https://img.shields.io/badge/Tests-524%20passing-2EA44F?logo=vitest&logoColor=white">
</p>

<p align="center">
  <a href="https://verawittyfx.com">Live ansehen</a> ·
  <a href="#features">Features</a> ·
  <a href="#architektur">Architektur</a> ·
  <a href="#engineering-highlights">Highlights</a> ·
  <a href="#zugang-zum-code">Code</a>
</p>

---

## Auf einen Blick

| | |
|---|---|
| **Status** | Seit 2026 produktiv im Einsatz |
| **Umfang** | ~123.000 Zeilen TypeScript · 89 Seiten · 244 API-Routen · 78 Datenmodelle |
| **Automatisierung** | 20 Cron-Jobs für Billing, Mahnwesen, Benachrichtigungen, Sync und Monitoring |
| **Qualität** | 524 Tests (Vitest), strikte TypeScript-Prüfung, Produktions-Build als Gate |
| **Plattformen** | Web (responsive) · iOS · Android (Capacitor) · Web-Push |

## Screenshots

<table>
  <tr>
    <td width="50%"><img src="assets/screenshots/home-desktop.png" alt="Startseite"></td>
    <td width="50%"><img src="assets/screenshots/pricing-desktop.png" alt="Preisseite"></td>
  </tr>
  <tr>
    <td align="center"><sub>Startseite</sub></td>
    <td align="center"><sub>Pakete & Checkout-Einstieg</sub></td>
  </tr>
  <tr>
    <td width="50%"><img src="assets/screenshots/lead-magnet-desktop.png" alt="Lead-Magnet-Landingpage"></td>
    <td width="50%"><img src="assets/screenshots/danke-desktop.png" alt="Danke-Seite mit Upsell"></td>
  </tr>
  <tr>
    <td align="center"><sub>Lead-Magnet (isolierte Subdomain, DSGVO-Double-Opt-In)</sub></td>
    <td align="center"><sub>Danke-Seite mit Cross- und Upsell</sub></td>
  </tr>
</table>

<p align="center">
  <img src="assets/screenshots/home-mobile.png" alt="Mobile Startseite" width="23%">
  <img src="assets/screenshots/pricing-mobile.png" alt="Mobile Preise" width="23%">
  <img src="assets/screenshots/lead-magnet-mobile.png" alt="Mobile Lead-Magnet" width="23%">
  <img src="assets/screenshots/danke-mobile.png" alt="Mobile Danke" width="23%">
</p>

## Features

**Lernen & Inhalte**
- Video-Kurse und Lernpfade mit Fortschritt und Zertifikaten (PDF)
- Sicheres HLS-Video-Streaming mit signierten URLs aus MinIO, inkl. Upload-Pipeline und Transcoding
- Tägliche Marktanalysen (Video + Chart-Snapshots) mit TradingView-Einbettung und Benachrichtigung
- Live-Calls über selbst gehostetes Jitsi mit Handheben, Viewer-Tracking und Aufzeichnungen
- Wirtschaftskalender mit Erinnerungen

**Community**
- Echtzeit-Chat mit Kanälen, Antworten, Pins, Bild- und Sprachnachrichten
- Ankündigungskanäle, Rollen- und Paket-basierte Zugriffsrechte, Discord-Integration

**Trading-Journal**
- Trades erfassen oder importieren (CSV/HTML, EU-Formate, UTF-16)
- Automatischer Sync mit MetaTrader 4/5 (Expert Advisor + Webhook) und cTrader
- Kennzahlen wie CRV, Trefferquote, Erwartungswert und Drawdown, Währungsumrechnung über EZB-Kurse

**Billing & Monetarisierung**
- Stripe-Integration für Abos, Lifetime-Käufe und Ratenzahlung
- Guest-Checkout mit Registrierungscode, Gutscheine, Rechnungen
- Automatisiertes Mahnwesen und laufender Abgleich mit Stripe
- Affiliate- und Referral-Programm mit Provisionsfreigabe
- Lead-Magnete auf eigener Subdomain mit Double-Opt-In, PDF-Auslieferung und Newsletter

**Sicherheit & Datenschutz**
- Verschlüsselung personenbezogener Daten auf Feldebene
- Zwei-Faktor-Authentifizierung und sichere Session-Verwaltung
- Mehrschichtiger Schutz vor Missbrauch und automatisierten Angriffen
- Einwilligungs-Protokoll (Art. 7 DSGVO), Datenexport und Löschung durch Nutzer

## Architektur

```mermaid
flowchart LR
  subgraph Clients
    W[Browser]
    M[iOS / Android<br/>Capacitor]
    MT[MetaTrader EA]
  end

  C[Caddy<br/>TLS, Routing, CSP]

  subgraph App[Docker Compose]
    A[Next.js App<br/>App Router + API]
    G[Lead-Magnet-Container<br/>guide.*]
    PG[(PostgreSQL)]
    R[(Redis<br/>Rate-Limits, Locks)]
    S[(MinIO<br/>Videos, PDFs, Medien)]
    J[Jitsi<br/>Live-Calls]
  end

  ST[Stripe]
  SM[SMTP / Resend]

  W & M --> C
  MT -->|Webhook| C
  C --> A & G & J
  A --> PG & R & S
  G --> PG & R & S
  A <-->|Webhooks| ST
  A & G --> SM
```

## Engineering-Highlights

- **Robuste Zahlungsabwicklung:** Webhook-Verarbeitung, die gegen Änderungen der Stripe-API abgesichert ist, plus automatischer Abgleich, der Abweichungen selbstständig erkennt und korrigiert.
- **Zuverlässige Benachrichtigungen:** E-Mail, Web-Push und In-App aus einer zentralen Pipeline, mit Wiederholung bei Fehlern und Versand, der Deployments übersteht.
- **Skalierbare Medien:** Video-Upload, Transcoding und HLS-Streaming mit signierten, zeitlich begrenzten URLs.
- **Isolierte Landingpages:** Öffentliche Marketing-Seiten laufen getrennt vom Kernsystem, aus demselben Build.
- **Eine Codebasis für Web, iOS und Android** über Capacitor, inklusive Push-Benachrichtigungen.

## Tech-Stack

| Bereich | Technologie |
|---|---|
| Frontend | Next.js 16 (App Router), React 18, TypeScript, Tailwind CSS, shadcn/ui |
| Backend | Next.js Route Handlers, Prisma 5, PostgreSQL 15, Redis 7 |
| Medien | MinIO (S3-kompatibel), HLS-Streaming, FFmpeg, pdf-lib |
| Zahlungen | Stripe (Payment Element, Checkout, Subscriptions, Webhooks) |
| Kommunikation | Nodemailer (SMTP), Resend, Web-Push, Jitsi |
| Mobile | Capacitor 7 (iOS & Android) |
| Infrastruktur | Docker Compose, Caddy 2, Cron-gesteuerte Jobs |
| Qualität | Vitest, Zod-Validierung, TypeScript strict |

## Zugang zum Code

Der Quellcode liegt in einem privaten Repository. Zugang gibt es auf Anfrage für Kunden, Partner und Recruiter.

**Kontakt:** [reinert-digital.de](https://reinert-digital.de)

---

<p align="center">
  Entwickelt von <a href="https://reinert-digital.de"><b>Reinert Digital Solutions</b></a><br>
  <sub>Showcase-Repository. Code, Inhalte und Marke VeraWittyFX sind urheberrechtlich geschützt.</sub>
</p>

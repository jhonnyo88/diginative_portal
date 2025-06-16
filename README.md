DigiNativa Portal: Teknisk Dokumentation & Vision
1. Syfte och Mål
Välkommen till diginative-portal. Detta repository innehåller koden för vår publika webbplats och SaaS-plattform. Dess syfte är att agera som det primära gränssnittet för våra kunder, från första intresse till färdig leverans.

Fas 1 (MVP): Skapa en enkel och professionell webbplats där en potentiell kund (t.ex. en kommunanställd) kan förstå vårt erbjudande, ladda upp ett strategidokument (PDF), ange grundläggande personaliseringsval (färger, persona) och skicka en beställning.

Fas 2 (SaaS-plattform): Utveckla portalen till ett fullfjädrad system med kundinloggning, orderhistorik, betalning och automatiserad leveranshantering.

2. Arkitektonisk Relation
Portalen är ett helt frikopplat frontend-system. Den är en klient till vårt backend-API och har inga direkta beroenden till devteam eller diginative_runtime_engine. Detta är en medveten design för att möjliggöra oberoende utveckling, testning och driftsättning.

graph TD
    subgraph diginative-portal (Detta Repo)
        A[Kundportal Frontend<br>(Next.js)]
    end

    subgraph Backend Services & Repos
        B[API Gateway & Order Orchestrator]
        C[AI Factory / devteam]
        D[diginative_runtime_engine]
    end

    A -- 1. Skickar order via HTTPS --> B
    B -- 2. Initierar jobb hos --> C
    C -- 3. Använder --> D
    B -- 4. Rapporterar status tillbaka till --> A

All kommunikation mellan denna portal och vår backend måste ske enligt det kontrakt som definieras i API_SPECIFICATION.md.

3. Teknisk Implementation & Val
Framework: Next.js (React)

Styling: Tailwind CSS

Hosting: Vercel eller liknande plattform.

State Management: React Context för enklare behov, Zustand vid behov av mer komplex state.

Formulärhantering: React Hook Form för robusta och validerade beställningsformulär.

4. Utvecklingsfokus
Fas 1: MVP Beställningsportal
Landningssida: En huvudsida som förklarar tjänsten.

Beställningssida (/bestall): Ett flerstegsformulär där användaren kan:

Ladda upp en PDF-fil.

Välja primär- och sekundärfärg med en färgväljare.

Välja en av de fördefinierade personorna från en lista.

Ange kontaktuppgifter.

Orderbekräftelse: En sida som visas efter att beställningen har skickats, med ett ordernummer och information om nästa steg.

API-integration: Implementera logik för att skicka beställningsdatan enligt API_SPECIFICATION.md.

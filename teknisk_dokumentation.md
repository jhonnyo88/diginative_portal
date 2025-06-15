StrategyPlay Portal: Teknisk Dokumentation & Vision
1. Syfte och Mål
Välkommen till strategyplay-portal. Detta repository innehåller koden för vår publika webbplats och framtida kundplattform. Dess syfte utvecklas i två faser:

Fas 1: Marknadsplats & Demo (Nuvarande fokus): Agera som vår primära marknadsföringskanal. Målet är att bygga en enkel, snabb och övertygande webbplats som förklarar vad StrategyPlay gör. Den ska också agera värd för vårt första publika demo-spel, "Gamifiering av Sveriges Digitaliseringsstrategi", för att visa kraften i vår plattform.

Fas 2: SaaS-plattform (Framtida vision): Utvecklas till en fullfjädrad, säker och skalbar kundportal där kunder kan registrera sig, hantera sina projekt, ladda upp dokument och få tillgång till sina färdiga spelupplevelser.

Detta repo är medvetet frikopplat från våra interna system för att kunna utvecklas och driftsättas oberoende.

2. Arkitektonisk Relation
Portalen är "ansiktet utåt" och har inga direkta kodberoenden till våra andra system. All kommunikation med vår backend sker via ett säkert API.

graph TD
    subgraph strategyplay-portal (Detta Repo)
        A[Hemsida/Plattform]
    end

    subgraph System & Repos
        B[AI Factory / devteam]
        C[strategyplay-runtime-engine]
        D[Genererad Spel-app för Demo]
    end

    subgraph Externa Tjänster
        E[API Gateway]
        F[Hosting (t.ex. Vercel)]
    end

    A -- länkar till/bäddar in --> D
    A -- kommunicerar med (Fas 2) --> E
    E -- tar emot jobb från (Fas 2) --> B
    B -- genererar --> D
    D -- använder --> C


Relation till övriga komponenter:

devteam (AI Factory): Portalen interagerar inte direkt med devteam. I Fas 2 kommer portalen att skicka förfrågningar till ett API, som i sin tur startar ett jobb hos devteam.

strategyplay-runtime-engine: Portalen har inget beroende till vår Runtime Engine. Det är de individuella spel-applikationerna (som "Sveriges Digitaliseringsstrategi") som använder motorn. Portalen behöver bara känna till URL:en till det färdiga spelet.

3. Teknisk Implementation & Val
Framework: Vi använder Next.js (eller ett liknande React-baserat ramverk som Astro). Detta ger oss fördelarna med:

Static Site Generation (SSG): Extremt snabba laddningstider för marknadssidorna, vilket är bra för SEO och användarupplevelse.

Server-Side Rendering (SSR) / API Routes: Möjligheten att enkelt bygga ut med dynamisk funktionalitet och serverlogik när vi går in i Fas 2.

Styling: Tailwind CSS för snabb och konsekvent design.

Hosting: Driftsätts på en modern hosting-plattform som Vercel eller Netlify för automatiserad CI/CD och global distribution.

4. Utvecklingsfokus & Roadmap
Fas 1: MVP Marknadsplats (Fokus Nu)
Landningssida: En huvudsida som förklarar värdeerbjudandet.

Om oss/Tekniken: En sida som beskriver visionen och hur systemet fungerar på en konceptuell nivå.

Demo-sida (/demo/sveriges-digitaliseringsstrategi): En dedikerad sida som antingen bäddar in eller länkar tydligt till vårt första publika spel. Denna sida ska rama in upplevelsen och förklara kontexten.

Fas 2: SaaS-portal (Vision)
Autentisering: System för användarregistrering och inloggning.

Projekt-Dashboard: En vy där inloggade användare kan se och hantera sina spel-projekt.

API-integration: Integration mot vårt API Gateway för att kunna starta nya spel-genereringsjobb.

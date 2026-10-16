# H4-bliep-pipeline-demo

Demo voor IT Trends: één wijziging reist van `development` via `staging` naar productie.

```text
pull request  ->  CI: tests  ->  merge  ->  Komodo ziet de nieuwe commit  ->  bouwt en rolt uit
```

| Branch        | Komodo-stack    | Poort |
|---------------|-----------------|-------|
| `development` | `bliep-dev`     | 8081  |
| `staging`     | `bliep-staging` | 8082  |
| `main`        | `bliep-prod`    | 8080  |

Promoveren doe je met een pull request: `development` naar `staging`, daarna `staging` naar `main`. Wat er draait, is altijd wat op de branch staat.

- `.github/workflows/ci.yml`: de tests, bij elke pull request en na elke merge.
- `komodo-resources.toml`: de drie stacks, en een Action die elke 30 seconden kijkt of er een nieuwe commit op hun branch staat.
- `compose.yaml`: dezelfde stack als in les 4. Komodo bouwt het image van de webshop zelf, er is geen registry nodig.
- `komodo/`: Komodo zelf, om de demo op je eigen laptop te draaien.

## De demo zelf draaien

Je hebt alleen Docker Desktop nodig. Komodo haalt de code van deze repo op, bouwt de webshop en rolt de drie omgevingen uit op je laptop.

**1. Start Komodo** in de hoofdmap van deze repo:

```text
docker compose -f komodo/compose.yaml up -d
```

**2. Log in** op [localhost:9120](http://localhost:9120) met gebruiker `admin` en wachtwoord `bliep-demo`. Onder *Servers* staat er één server, `Local`.

**3. Maak de stacks aan.** Ga naar *Syncs*, maak een nieuwe Resource Sync, kies als bron de UI, plak de inhoud van `komodo-resources.toml` en voer de sync uit. Je krijgt drie stacks (`bliep-dev`, `bliep-staging`, `bliep-prod`) en een Action `bliep-uitrollen`.

**4. Wacht een minuut of twee.** Binnen 30 seconden ziet de Action dat de stacks nog niet draaien, en rolt ze ze één voor één uit. De eerste keer duurt het bouwen wat langer.

Open [localhost:8081](http://localhost:8081) (development), [localhost:8082](http://localhost:8082) (staging) en [localhost:8080](http://localhost:8080) (productie). Onderaan elke pagina staat de omgeving en de korte code van de commit.

Merge je daarna iets op een branch, dan rolt Komodo binnen 30 seconden die ene omgeving opnieuw uit. De andere twee blijven ongewijzigd.

**Stoppen:** zet in Komodo de Action `bliep-uitrollen` uit (anders start ze de stacks opnieuw), stop de drie stacks, en daarna Komodo zelf met `docker compose -f komodo/compose.yaml down`. Met `down -v` gooi je ook alles weg wat Komodo bewaard heeft.

Komodo is alleen bereikbaar op je eigen laptop. Daarom mogen de wachtwoorden in `komodo/komodo.env` gewoon in deze repo staan.

Het volledige draaiboek staat in het lespakket van les 4.

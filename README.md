# H4-bliep-pipeline-demo

Demo voor IT Trends: één wijziging reist van `development` via `staging` naar productie.

```text
pull request  ->  CI: tests  ->  merge  ->  CI: image bouwen en pushen naar GHCR  ->  Komodo rolt uit
```

| Branch        | Image-label    | Komodo-stack    | Poort |
|---------------|----------------|-----------------|-------|
| `development` | `:development` | `bliep-dev`     | 8081  |
| `staging`     | `:staging`     | `bliep-staging` | 8082  |
| `main`        | `:production`  | `bliep-prod`    | 8080  |

Promoveren doe je met een pull request: `development` naar `staging`, daarna `staging` naar `main`.

- `.github/workflows/ci-cd.yml`: tests bij elke pull request, en na een merge ook het image bouwen en pushen.
- `komodo-resources.toml`: de drie stacks en een procedure die elke 30 seconden naar nieuwe images kijkt.
- `compose.yaml`: dezelfde stack als in les 4, maar met een kant-en-klaar image in plaats van `build`.
- `komodo/`: Komodo zelf, om de demo op je eigen laptop te draaien.

## De demo zelf draaien

Je hebt alleen Docker Desktop nodig. Komodo haalt de images van deze repo op, en rolt de drie omgevingen uit op je laptop.

**1. Start Komodo** in de hoofdmap van deze repo:

```text
docker compose -f komodo/compose.yaml up -d
```

**2. Log in** op [localhost:9120](http://localhost:9120) met gebruiker `admin` en wachtwoord `bliep-demo`. Onder *Servers* staat er één server, `Local`.

**3. Maak de stacks aan.** Ga naar *Syncs*, maak een nieuwe Resource Sync, kies als bron de UI, plak de inhoud van `komodo-resources.toml` en voer de sync uit. Je krijgt drie stacks (`bliep-dev`, `bliep-staging`, `bliep-prod`) en een procedure `bliep-auto-update`.

**4. Deploy de drie stacks** één keer met de hand. Daarna volgt Komodo zelf de nieuwe images.

Open [localhost:8081](http://localhost:8081) (development), [localhost:8082](http://localhost:8082) (staging) en [localhost:8080](http://localhost:8080) (productie). Onderaan elke pagina staat de omgeving en de korte code van de commit.

**Stoppen:** stop de drie stacks in Komodo, en daarna Komodo zelf met `docker compose -f komodo/compose.yaml down`. Met `down -v` gooi je ook alles weg wat Komodo bewaard heeft.

Komodo is alleen bereikbaar op je eigen laptop. Daarom mogen de wachtwoorden in `komodo/komodo.env` gewoon in deze repo staan.

Het volledige draaiboek staat in het lespakket van les 4.

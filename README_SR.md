# DockJump — Go + Cobra Codex paket

**60 kompletnih promptova za CLI aplikaciju koja upravlja Docker kontejnerima preko postojećeg SSH configa i jump hosta.**

Verzija paketa: 1.0.0 • 28.09.2026.
Radni naziv: **DockJump**, komanda `dockjump`. Dostupnost naziva/domena/žiga nije proveravana.

Ovo je zaseban CLI paket za istu ideju kao prethodna desktop aplikacija. Sadrži promptove, arhitekturu, ugovor komandi i funkcionalan tracker. Izvorni kod i gotov `dockjump` binarni fajl nastaju izvršavanjem promptova u tvom projektu.

## Stack i način rada

- **Go + Cobra**, jedan Go modul i jedan CLI binarni fajl.
- **Native OpenSSH** koristi `~/.ssh/config`, `ProxyJump`, `IdentityFile`, SSH agent i `known_hosts`.
- Cilj je Linux server sa Docker Engine-om; lokalni CLI radi na **Linuxu i macOS-u**.
- Predviđeni buildovi: `linux/amd64`, `linux/arm64`, `darwin/amd64`, `darwin/arm64`, uz zasebne stvarne runtime provere.
- Tabele i čitljiv tekst za terminal; **JSON/JSONL za jq i CI**.
- Završena aplikacija zahteva binarni fajl i OpenSSH. Docker i jq na lokalnom računaru nisu obavezni.
- Python 3.10+ koristi samo razvojni tracker iz ovog paketa; nije zavisnost gotovog CLI-ja.

## Planirane komande

Ovo su primeri interfejsa koji promptovi implementiraju; komande nisu već instalirane raspakivanjem ZIP-a. `app-server` je alias iz tvog SSH configa, a `api` je primer imena kontejnera.

```bash
# Pronađi SSH aliase i proveri vezu.
dockjump hosts discover
dockjump hosts add app-server --group production
dockjump --host app-server hosts check

# Pregled svih kontejnera i JSON za jq.
dockjump --host app-server containers ls --all
dockjump --host app-server containers ls --all --output json |
  jq -r '.items[] | [.name, .state, .image] | @tsv'

# Detalji, logovi uživo i statistika.
dockjump --host app-server containers inspect api
dockjump --host app-server containers logs api --tail 200 --follow
dockjump --host app-server containers stats --watch --interval 5s

# Upravljanje: eksplicitna namera + potvrda.
dockjump --host app-server --write containers restart api

# Bez interaktivnog pitanja, pogodno za skripte.
dockjump --host app-server --write --yes containers restart api

# Pravi interaktivni shell unutar kontejnera.
dockjump --host app-server --write --yes exec -it api -- /bin/sh

# Kompletiranje komandi.
dockjump completion bash
dockjump completion zsh
```

Sva pravila i izlazni kodovi definisani su u `codex/docs/CLI_CONTRACT.md`. Aplikacija sama parsira Docker JSON; `jq` služi samo ako želiš dodatnu obradu njenog izlaza.

## Početak rada u Codex-u

1. Raspakuj ZIP u novi projektni folder.
2. Otvori taj folder u lokalnom Codex okruženju.
3. Pročitaj `CODEX_START.md` i pošalji početni tekst iz njega.
4. Izvršavaj po jedan prompt; tracker pamti sledeći korak.

```bash
python3 codex/scripts/track.py validate
python3 codex/scripts/track.py next
python3 codex/scripts/track.py show 001
```

Za novi razgovor sa Codex-om upotrebi isti početni tekst. On prvo čita stanje pa nastavlja aktivan ili blokiran korak. Nemoj ponovo raspakivati početni `state.json` preko svog napretka.

Ako već imaš projekat, sačuvaj postojeći kod i pravila. Kopiraj paket u odgovarajući koren bez prepisivanja postojećeg `codex/`; `AGENTS.md` spoji sa postojećim pravilima. Ne unosi CLI paket preko prethodne desktop implementacije osim ako svesno radiš migraciju; ovaj paket je zamišljen kao zaseban Go projekat.

## Šta se dobija kroz faze

| Promptovi | Rezultat |
|---|---|
| 001–008 | Cobra osnova, konfiguracija, tabele/JSON, greške i pravila |
| 009–018 | SSH config, jump hostovi, ključevi, Go procesi i stvarna provera veze |
| 019–030 | Kontejneri, detalji, logovi, stats/watch, događaji i Compose pregled |
| 031–038 | Dry-run, start/stop/restart, batch ishodi, resursi i Compose akcije |
| 039–046 | Exec/TTY, signali, pipe obrada, grupe hostova i completion |
| 047–060 | Testovi, CI, GoReleaser, Linux/macOS provere i predaja |

MVP za pregled je do **030**, upravljanje do **038**, funkcionalno kompletan CLI do **046**. Ostali koraci proveravaju ponašanje i pripremaju binarne pakete.

## Praćenje izvršavanja

Tracker beleži `pending`, `in_progress`, `blocked` i `done`, zavisnosti, preporučeni reasoning, dokaze, komande provere, vreme i istoriju promena. Dopušta jedan aktivan prompt, sprečava preskakanje i proverava hash dokaza.

Komande su `next`, `show`, `start`, `done`, `block`, `reopen`, `summary`, `validate` i `render`. Uputstvo: `codex/docs/TRACKING.md`.

Reasoning `medium`/`high` je preporuka po zadatku. Tracker je prikazuje, ali **ne menja model ili reasoning u tvom editoru i ne pokreće Codex automatski**.

## Sadržaj paketa

| Fajl/folder | Namena |
|---|---|
| `CODEX_START.md` | Početni tekst i nastavak sesije |
| `AGENTS.md` | Pravila implementacije |
| `codex/prompts/` | 60 zasebnih punih promptova |
| `codex/ALL_PROMPTS.md` | Svi promptovi objedinjeni |
| `codex/manifest.json` | Redosled, zavisnosti, reasoning i hash |
| `codex/scripts/track.py` | Razvojni tracker, Python standardna biblioteka |
| `codex/tracking/` | Stanje, pregled, istorija i dokazi |
| `codex/docs/CLI_CONTRACT.md` | Komande, flags, JSON i exit codes |
| `codex/docs/ARCHITECTURE.md` | Go organizacija i SSH/Docker odluke |
| `codex/docs/PLATFORM_MATRIX_TEMPLATE.md` | Stvarne Linux/macOS provere |
| `codex/examples/` | SSH primer i demonstracioni CLI izlaz |
| `PACK_VERIFICATION.md` | Provere samog paketa |

Live SSH provere zahtevaju tvoje test okruženje. Native provere zahtevaju odgovarajući OS/runner; cross-build ne znači da je runtime test prošao. Instaliranje CLI-ja ne menja dozvole korisnika na udaljenom Docker serveru.

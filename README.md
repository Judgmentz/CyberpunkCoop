# Cyberpunk Coop

Mod cooperativa a 2 giocatori per Cyberpunk 2077, basata su [CyberpunkMP](https://github.com/tiltedphoques/CyberpunkMP).
Uno ospita la partita (host), l'altro si collega (client). La storia segue quella dell'host.

**Serve:** Cyberpunk 2077 versione **2.5** (Steam, GOG o Epic). Entrambi con **lo stesso pacchetto**.

**[Scarica l'ultima versione](../../releases/latest)** (file `CyberpunkCoop-....zip`)

- [Windows](#windows)
- [Linux (Steam + Proton)](#linux-steam--proton)
- [Collegarsi via internet](#collegarsi-via-internet)
- [In gioco](#in-gioco)
- [Aggiornamenti](#aggiornamenti)
- [Problemi](#problemi)

---

## Windows

### Installare

1. Scarica lo zip ed estrailo in una cartella qualsiasi, per esempio il Desktop. Non dentro il gioco.
2. Avvia `CyberpunkCoop.exe`. Se Windows dice "PC protetto": **Ulteriori informazioni**, poi **Esegui comunque**.
3. L'app trova il gioco da sola. Se non lo trova, scegli la cartella di Cyberpunk 2077 (quella che contiene `bin`).
4. Premi **Installa la mod**. La prima volta copia i salvataggi in `Documenti\CyberpunkCoop-backup`.
5. Scrivi il tuo nome a sinistra.

### Ospitare (host)

1. Scegli **Ospito io**. Il server parte da solo.
   - Se manca .NET 8, l'app lo dice: Impostazioni, **Installa .NET 8**.
   - Se Windows chiede se consentire **Server.Loader** alle reti, premi **Consenti**.
2. Premi **Copia** e manda l'indirizzo all'amico.
3. Scegli il salvataggio da caricare nel menu sotto (o lascia "il piu' recente").
4. Premi **GIOCA**. Entra per primo: l'amico si collega dopo.

### Collegarsi (client)

1. Scegli **Mi unisco** e incolla l'indirizzo dell'host.
2. Scegli il salvataggio da caricare, poi **GIOCA** quando l'host e' gia' in partita.
   Il gioco carica il salvataggio e si collega da solo.

---

## Linux (Steam + Proton)

Provato su CachyOS con Steam e Proton, come client. Ospitare da Linux non e' stato provato: chi ospita usi Windows.

### Installare

Hai due modi, scegline uno.

**A. Con l'app (serve Wine)**

```bash
sudo pacman -S wine          # Arch / CachyOS (su Ubuntu/Debian: sudo apt install wine)
cd ~/CyberpunkCoop/CyberpunkCoop
wine CyberpunkCoop.exe
```

L'app trova il gioco nella libreria Steam (`Z:\home\...\Steam\steamapps\common\Cyberpunk 2077`).
Premi **Installa la mod**, poi **Mi unisco**, incolla l'indirizzo e premi **GIOCA**:
su Linux GIOCA non avvia il gioco ma **copia negli appunti le opzioni di avvio** per Steam.

**B. Senza app (script)**

```bash
cd ~/CyberpunkCoop/CyberpunkCoop
bash installa-linux.sh
```

Lo script trova il gioco nelle librerie Steam, copia la mod e stampa le opzioni di avvio.
Se il gioco e' altrove: `bash installa-linux.sh "/percorso/Cyberpunk 2077"`.

### Opzioni di avvio in Steam

Tasto destro su Cyberpunk 2077, **Proprieta'**, **Opzioni di avvio**:

```
WINEDLLOVERRIDES="winmm,version=n,b" %command% --online --ip=IP_HOST --port=11778 --name=TuoNome
```

- `IP_HOST` e' l'indirizzo che ti manda l'host.
- Il nome senza spazi.
- Aggiungi `--skipStartMenu --autoconnect` per caricare l'ultimo salvataggio e collegarti da solo.
- `WINEDLLOVERRIDES` e' obbligatorio: senza, Proton non carica la mod.

Avvia il gioco da Steam quando l'host e' gia' in partita.

---

## Collegarsi via internet

L'amico deve poter raggiungere il PC dell'host sulla porta **UDP 11778**.

**Il modo piu' semplice: Tailscale** (gratis, entrambi lo installano).

- Windows: scarica da [tailscale.com/download](https://tailscale.com/download) e accedi.
- Linux:
  ```bash
  sudo pacman -S tailscale     # o: curl -fsSL https://tailscale.com/install.sh | sh
  sudo systemctl enable --now tailscaled
  sudo tailscale up
  ```
- L'host, dalla [console di Tailscale](https://login.tailscale.com/admin/machines), condivide il suo PC con l'amico
  (tre puntini sul PC, **Share...**).
- L'app dell'host mostra da sola l'indirizzo Tailscale (`100.x.x.x`): e' quello da mandare.

**Senza VPN:** l'host apre sul router la porta **UDP 11778** verso il suo PC e manda il suo IP pubblico.
Funziona solo se l'operatore da' un IP pubblico vero (con CGNAT non va).

---

## In gioco

- Storia, missioni, scelte e diario seguono l'host.
- Nemici, colpi e morti sono condivisi. Si vedono auto, armi in mano e spari dell'altro.
- Ora del giorno e meteo seguono l'host.
- Se l'altro e' a piu' di 150 m, il client viene portato accanto all'host.
- Chat (tasto della chat):
  - `/tp` raggiungi l'altro giocatore;
  - `/sali` sali come passeggero nella sua auto (entro 8 m);
  - `/scendi` (o F) scendi.
- Se la connessione cade, il gioco riprova da solo per 2 minuti.

**Salvataggi del client:** mentre e' collegato segue la storia dell'host, quindi se salva il salvataggio
contiene i progressi dell'host. Per tornare alla tua partita da solo usa un salvataggio fatto prima
(copia in `Documenti\CyberpunkCoop-backup`).

---

## Aggiornamenti

All'avvio l'app controlla se c'e' una versione nuova. Se c'e', in alto compare **Versione nuova** con
il pulsante **Aggiorna**: scarica, sostituisce i file e si riavvia. Poi premi **Aggiorna la mod** (a gioco chiuso).
Aggiornate entrambi: con versioni diverse compare un avviso in chat.

Con lo script su Linux: scarica lo zip nuovo e rilancia `bash installa-linux.sh`.

---

## Problemi

| Problema | Cosa fare |
|---|---|
| Non si collega | L'host e' gia' in partita? L'indirizzo e' giusto? Tailscale acceso su entrambi? |
| `/tp` dice "Nessun altro giocatore in vista" | Il personaggio dell'altro non e' ancora comparso: fallo scrivere a lui. |
| Linux: il gioco crasha mentre carica (NVIDIA) | Togli override DLSS / frame generation (`PROTON_DLSS_UPGRADE`), ray reconstruction e path tracing. Prova senza `--online`: se crasha anche cosi', la mod non c'entra. |
| Linux: la mod non parte | Controlla `WINEDLLOVERRIDES="winmm,version=n,b"` nelle opzioni di avvio. |
| Il primo avvio e' lento | Normale: il gioco ricompila gli script. |
| Tornare al gioco normale | Nell'app: Impostazioni, **Disinstalla la mod**. |

---

## Licenze

CyberpunkMP e le dipendenze (RED4ext, redscript, Codeware, ArchiveXL, TweakXL, Input Loader)
hanno le proprie licenze, incluse nello zip nella cartella `licenze`.

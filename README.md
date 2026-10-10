# Team - Impero Di Tibet
 
Datapack per **Minecraft Java 26.2** che mostra una classifica fluttuante con tre righe (**team1**, **team2**, **team3**) e un contatore accanto a ciascuna. Ogni volta che esegui una function, il contatore sale di 1 e le righe si riordinano da sole in base ai punti.
 
```
team1: 5
team3: 3
team2: 1
```
 
## Caratteristiche
 
- Testo volante (`text_display`) che ruota sempre verso il giocatore
- Contatori salvati in uno scoreboard, quindi il numero si aggiorna da solo
- Classifica ordinata automaticamente dal punteggio più alto al più basso
- In caso di pareggio vale un ordine fisso: team1, team2, team3
- Lo scoreboard si crea da solo al caricamento e `/reload` non azzera i punti
## Requisiti
 
- Minecraft Java Edition **26.2** (formato datapack `107.1`)
- Permessi per eseguire `/function` (operatore o comando da command block)
## Installazione
 
1. Metti il datapack nella cartella `datapacks` del mondo:
   - singleplayer: `saves/<nome mondo>/datapacks/`
   - server: `<cartella del server>/world/datapacks/`
2. Entra nel mondo e fai `/reload`.
3. Controlla con `/datapack list` che `titolo-volante` sia attivo.
> **Attenzione:** il file `pack.mcmeta` deve stare nella radice dello zip o della cartella del datapack. Il pulsante "Download ZIP" di GitHub mette tutto dentro un'altra cartella, quindi in quel caso estrai lo zip e copia dentro `datapacks` la cartella che contiene direttamente `pack.mcmeta`.
 
## Utilizzo
 
1. Mettiti dove vuoi la classifica ed esegui `/function classifica:spawn`. Il testo compare 2 blocchi sopra di te.
2. Dai punti con le function qui sotto, anche da command block o da altri datapack.
| Comando | Cosa fa |
| --- | --- |
| `/function classifica:spawn` | Crea il testo volante (cancella quello vecchio se c'è già) |
| `/function classifica:team1` | +1 punto ai team1 |
| `/function classifica:team2` | +1 punto ai team2 |
| `/function classifica:team3` | +1 punto a team3 |
| `/function classifica:azzera` | Rimette tutti i punteggi a 0 |
| `/function classifica:rimuovi` | Elimina il testo volante |
 
## Come funziona
 
1. Le function `team1`, `team2` e `team3` aggiungono 1 al punteggio della squadra nello scoreboard `contatori`.
2. Subito dopo chiamano `classifica:ordina`, che confronta i tre punteggi e assegna a ogni squadra una posizione nello scoreboard `ordine`.
3. In base alle posizioni, `ordina` riscrive il testo del `text_display` con le righe nell'ordine giusto. I numeri nel testo sono componenti `score`, quindi leggono direttamente il valore dello scoreboard.
Il testo si riconosce dal tag `classifica`.
 
## Personalizzazione
 
- **Altezza del testo:** in `spawn.mcfunction` cambia `~ ~2 ~` (il secondo valore è l'altezza in blocchi).
- **Colori:** nelle righe di `ordina.mcfunction` cambia il valore di `color` (ad esempio `light_purple`, `aqua`, `gray`).
- **Nomi mostrati:** in `ordina.mcfunction` cambia i testi `"team1: "`, `"team2: "` e `"team3: "`. Il nome interno usato dallo scoreboard (`#team1`, `#team2`, `#team3`) può restare uguale.
- **Numero di righe:** l'ordinamento è scritto per esattamente 3 squadre. Per aggiungerne altre bisogna estendere il calcolo delle posizioni e le combinazioni in `ordina.mcfunction`.
## Struttura del progetto
 
```
titolo-volante/
├── pack.mcmeta
└── data/
    ├── minecraft/tags/function/
    │   └── load.json              # esegue setup a ogni caricamento
    └── classifica/function/
        ├── setup.mcfunction       # crea gli scoreboard e i valori iniziali
        ├── spawn.mcfunction       # crea il testo volante
        ├── ordina.mcfunction      # calcola le posizioni e riscrive il testo
        ├── team1.mcfunction       # +1 ai team1
        ├── team2.mcfunction       # +1 ai team2
        ├── team3.mcfunction       # +1 a team3
        ├── azzera.mcfunction      # azzera i punteggi
        └── rimuovi.mcfunction     # elimina il testo
```
 
## Problemi comuni
 
- **Il testo non compare:** controlla che il datapack sia attivo con `/datapack list` e rilancia `/function classifica:spawn`.
- **Il numero non cambia:** verifica che lo scoreboard esista con `/scoreboard objectives list`. Se manca, fai `/reload`.
- **Le righe non si riordinano:** usa le function `team1`, `team2` e `team3` e non modificare lo scoreboard a mano, altrimenti l'ordine si aggiorna solo al punto successivo.
- **Più testi sovrapposti:** usa sempre `spawn`, che cancella quello vecchio prima di crearne uno nuovo.

## Licenza

Progetto distribuito con licenza [MIT](LICENSE).
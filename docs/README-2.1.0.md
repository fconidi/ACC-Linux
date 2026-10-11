# Advanced Comics Converter — ACC Linux 2.1.0

> [Versione italiana](#versione-italiana) | [English version](#english-version)

## Download Linux

Download e supporto / Download and support — Buy Me a Coffee:

https://buymeacoffee.com/fconidi/e/489972

<img width="230" height="230" alt="ACC-LINUX-QR" src="https://github.com/user-attachments/assets/6384d315-ffa0-4863-b385-319660e02070" />

## Versione italiana

Advanced Comics Converter **2.1.0** converte ricorsivamente archivi CBZ/CBR in PDF,
con interfaccia Zenity oppure da terminale. Richiede Linux, Bash >= 4.1 e img2pdf >= 0.4.4.

### Installazione e avvio

Scarica la distribuzione Linux dal link Buy Me a Coffee qui sopra.
Su Debian, Ubuntu e derivate puoi installare il pacchetto 2.1.0 dalla
cartella in cui lo hai scaricato:

```bash
sudo apt install ./advanced-comics-converter_2.1.0_all.deb
advanced-comics-converter --version
```

APT gestisce anche le dipendenze disponibili nei repository configurati.
Per preparare manualmente le dipendenze:

```bash
sudo apt install imagemagick img2pdf pdftk-java unzip zenity parallel file python3
# Per CBR, installare almeno un estrattore disponibile nella propria distribuzione:
sudo apt install unrar
```

Sono supportati `unrar`, `rar`, `unar` e `7z`, in questo ordine. `rar` serve anche
per tentare la riparazione di una copia privata degli archivi corrotti. Per RAR
compressi, il pacchetto che fornisce `7z` deve includere il decoder RAR.

Dopo l'installazione del pacchetto, avviare `advanced-comics-converter` dal terminale
o dal menu applicazioni.

1. Avvia **Advanced Comics Converter** dal menu oppure esegui `advanced-comics-converter`.
2. Seleziona la cartella che contiene i fumetti: vengono cercate anche le sottocartelle.
3. Segui la barra di avanzamento; puoi annullare il lavoro dalla finestra.
4. Controlla il riepilogo dei convertiti, saltati e falliti.
5. Apri i PDF creati accanto agli archivi originali.

Esempio: `/home/utente/Fumetti/Volume 01.cbz` produce
`/home/utente/Fumetti/Volume 01.pdf`.

Per avviare gli script del pacchetto scaricato, conserva `lib.sh` accanto
ad `advanced-comics-converter.sh`, entra nella loro cartella ed esegui:

```bash
bash ./advanced-comics-converter.sh
```

### Uso da terminale

`--directory` abilita la modalità senza finestre; senza questa opzione viene
mostrata la selezione della cartella tramite Zenity.

```bash
advanced-comics-converter --directory /percorso/fumetti --jobs 2
advanced-comics-converter --directory /percorso/fumetti --log-file /tmp/conversione.log
advanced-comics-converter --directory /percorso/fumetti --jpeg-quality 85
advanced-comics-converter --help
```

| Opzione | Comportamento |
| --- | --- |
| `--directory DIR` | Scansione ricorsiva senza interfaccia grafica. |
| `--jobs N` | Numero positivo di archivi elaborati contemporaneamente; predefinito: 80% delle CPU disponibili. |
| `--log-file FILE` | Percorso del log, relativo alla cartella di avvio oppure assoluto. |
| `--jpeg-quality 1..100` | Ricodifica esplicita in JPEG con perdita; predefinito: conservazione delle immagini originali. |
| `--version` | Visualizza la versione. |

Il parallelismo predefinito controlla il numero di processi, non impone un limite
all'uso della CPU. Per contenere la RAM usare, ad esempio, `--jobs 2`.
In modalità JPEG, `MAGICK_THREAD_LIMIT` vale 1 se non già configurato.

### Conversione, output e sicurezza dei file

- Il formato ZIP/RAR viene rilevato dal contenuto: gli archivi originali non
  vengono rinominati né modificati. Le estensioni maiuscole sono accettate.
- Sottocartelle e nomi delle pagine sono conservati durante l'estrazione;
  l'ordine naturale del percorso relativo distingue `2.png` da `10.png`.
- Ogni immagine produce una pagina. GIF animate e TIFF multipagina utilizzano
  il primo fotogramma. File README e metadati macOS vengono ignorati.
- img2pdf genera un unico documento mantenendo le immagini originali.
  PDFtk verifica la leggibilità e il conteggio delle pagine, senza unire PDF intermedi.
- Il PDF `<nome_archivio>.pdf` compare accanto all'archivio solo dopo la verifica.
  Errori di estrazione, immagini danneggiate o pagine mancanti fanno fallire
  l'intero archivio. Un eventuale PDF precedente resta intatto in caso di errore.
- Un PDF esistente e leggibile viene saltato; uno vuoto o invalido viene rigenerato.
  Se più archivi producono lo stesso percorso PDF, vengono segnalati come conflitti.
- I temporanei sono privati e distinti per ogni lavoro. Le cartelle preesistenti
  `<nome>_images` non vengono utilizzate o eliminate. I membri ZIP con percorsi
  non sicuri, collegamenti simbolici o nomi duplicati vengono rifiutati.
- Annullare la barra Zenity interrompe i processi della conversione. I PDF già
  completati restano disponibili; gli output parziali vengono eliminati.

### Log e codici di uscita

Il log predefinito è `comics_converter_errors.log` nella cartella da cui è stato
avviato il programma. Il percorso resta stabile, il contenuto viene aggiunto e
include archivio elaborato, stderr degli strumenti e riepilogo dei risultati.

| Codice | Significato |
| --- | --- |
| `0` | Tutti gli archivi sono stati convertiti o saltati perché già validi; anche cartella vuota. |
| `1` | Almeno un archivio fallito, dipendenza mancante o errore operativo. |
| `2` | Argomenti non validi. |
| `130` | Annullamento dell'utente oppure segnale di interruzione. |

### Problemi comuni

| Problema | Cosa controllare |
| --- | --- |
| Dipendenza mancante | Installa il pacchetto richiesto e ripeti l'avvio. |
| CBR non leggibile | Installa un estrattore supportato; se usi 7z, verifica che includa il decoder RAR. |
| Finestra Zenity assente | Avvia da una sessione grafica oppure usa `--directory` per lavorare dal terminale. |
| PDF non creato | Leggi il log, controlla integrità dell'archivio, permessi e spazio libero. |
| Nome PDF in conflitto | Due archivi producono lo stesso nome: assegna nomi distinti agli archivi prima di riprovare. |
| Conversione pesante | Riduci i processi contemporanei con `--jobs 1` o `--jobs 2`. |

I PDF esistenti e leggibili vengono saltati. Per riconvertirne uno, spostalo
prima in una cartella di backup e riesegui ACC. L'app non offre un'opzione
`--force`. Conserva gli archivi originali per ripetere la conversione.

I temporanei delle immagini e il PDF richiedono spazio libero: il fabbisogno
dipende dalle immagini contenute negli archivi e dal numero di lavori simultanei.
Per segnalare un problema indica versione dell'app, distribuzione Linux,
comando usato ed errore presente nel log.

## English version

Advanced Comics Converter **2.1.0** recursively converts CBZ/CBR archives to PDF
through Zenity or a headless CLI. Requires Linux, Bash >= 4.1 and img2pdf >= 0.4.4.

### Installation and launch

Download the Linux distribution from the Buy Me a Coffee link above.
On Debian, Ubuntu and derivatives, install the 2.1.0 package from its download folder:

```bash
sudo apt install ./advanced-comics-converter_2.1.0_all.deb
advanced-comics-converter --version
```

APT also handles dependencies available in your configured repositories.
To prepare dependencies manually:

```bash
sudo apt install imagemagick img2pdf pdftk-java unzip zenity parallel file python3
# For CBR, install at least one extractor available in your distribution:
sudo apt install unrar
```

Supported extractors, in order: `unrar`, `rar`, `unar`, `7z`. After normal extraction
fails, `rar` can attempt repair on a private copy of the archive. Compressed RAR
files require a `7z` package with a RAR decoder when using that backend.
Launch the installed `advanced-comics-converter` command or application menu entry.
1. Open **Advanced Comics Converter** from the application menu, or run `advanced-comics-converter`.
2. Select the comics folder; subfolders are searched recursively.
3. Follow the progress bar or cancel from the window.
4. Check the converted/skipped/failed summary.
5. Open the PDFs saved beside the original archives.

Example: `/home/user/Comics/Volume 01.cbz` produces
`/home/user/Comics/Volume 01.pdf`.

To run downloaded scripts directly, keep `lib.sh` beside
`advanced-comics-converter.sh`, open their folder and run:

```bash
bash ./advanced-comics-converter.sh
```

### Headless options

```bash
advanced-comics-converter --directory /path/to/comics --jobs 2
advanced-comics-converter --directory /path/to/comics --log-file /tmp/conversion.log
advanced-comics-converter --directory /path/to/comics --jpeg-quality 85
advanced-comics-converter --help
```

| Option | Behavior |
| --- | --- |
| `--directory DIR` | Recursive conversion without GUI dialogs; otherwise Zenity selects the folder. |
| `--jobs N` | Positive number of concurrent archives; default: 80% of available CPUs. |
| `--log-file FILE` | Absolute path or path relative to the invocation directory. |
| `--jpeg-quality 1..100` | Explicit lossy JPEG reencoding; default preserves original image data. |
| `--version` | Print the version. |

The default concurrency controls job count rather than CPU utilization. Use
`--jobs 2`, for example, to reduce memory demand. JPEG conversion defaults
`MAGICK_THREAD_LIMIT` to 1 unless already configured.

### Conversion and file preservation

- ZIP/RAR signatures select the extractor without renaming or modifying input
  archives. Uppercase extensions, spaces, quotes and newline filenames are supported.
- Nested paths remain intact and pages use natural relative-path order.
  Each image produces one page; animated GIFs and multipage TIFFs use the first
  frame. README files and macOS metadata are ignored.
- img2pdf creates one document from the original images. PDFtk checks readability
  and page count; no intermediate page PDFs or merge step are used.
- `<archive_name>.pdf` is published next to its archive only after validation.
  An extraction error, damaged image or missing page fails the whole archive.
  Failed regeneration preserves an existing PDF byte for byte.
- Readable existing PDFs are skipped; empty/invalid PDFs are regenerated.
  Archives sharing the same output path fail as conflicts.
- Private, unique temporary directories protect existing `<name>_images` folders.
  ZIP members with unsafe paths, symbolic links or duplicate names are rejected.
- Cancelling Zenity progress stops conversion children and removes incomplete
  outputs. Previously completed PDFs remain available.

### Logs and exit codes

The default `comics_converter_errors.log` stays in the invocation directory.
Logs are appended and include archive paths, tool stderr, and conversion summaries.

| Code | Meaning |
| --- | --- |
| `0` | Every archive converted or skipped as valid; also an empty input directory. |
| `1` | An archive failed, a dependency is missing, or an operational error occurred. |
| `2` | Invalid arguments. |
| `130` | User cancellation or interruption signal. |

### Troubleshooting

| Problem | What to check |
| --- | --- |
| Missing dependency | Install the required package and launch again. |
| Unreadable CBR | Install a supported extractor; a 7z backend needs its RAR decoder. |
| No Zenity window | Use a graphical session or pass `--directory` for terminal operation. |
| No PDF created | Read the log and check archive integrity, permissions and free space. |
| Conflicting output names | Give the source archives distinct names before retrying. |
| Heavy conversion | Reduce concurrency with `--jobs 1` or `--jobs 2`. |

Readable existing PDFs are skipped. To recreate one, move it to a backup
folder first and rerun ACC. There is no `--force` option. Keep source archives
so you can convert them again.

Temporary images and the output PDF need free disk space; requirements depend
on archive contents and concurrent jobs. When reporting an issue, include the
app version, Linux distribution, command used and relevant log error.

## Video

Video dimostrativo della versione precedente / Previous-version demonstration:

[![ACC](https://img.youtube.com/vi/gxKtQuFBChE/hqdefault.jpg)](https://www.youtube.com/watch?v=gxKtQuFBChE)

## License and author

GPL-3.0-or-later; see [copyright](copyright).
Franco Conidi aka Edmond — [francoconidi.it](https://francoconidi.it),
[SysLinuxOS](https://syslinuxos.com).

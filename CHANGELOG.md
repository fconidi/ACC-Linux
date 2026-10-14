# Changelog

## [2.3.0] - 2026-10-10

### Versione italiana

- Protetti i log da collisioni con archivi, PDF e alias; limitati output e lavori del supervisore, migliorato l'annullamento.
- Aggiunte preferenze XDG, filtri e durata dei risultati, preparazione GTK asincrona e messaggi in cinque lingue.
- Documentata la nuova finestra GTK con una schermata della versione 2.3.0.

### English version

- Protect logs from source/PDF collisions and aliases; bound supervisor output and jobs, improve cancellation.
- Add XDG preferences, result filters and duration, background GTK preparation and five UI languages.
- Document the new GTK window with a version 2.3.0 screenshot.

## [2.2.1] - 2026-10-10

### Versione italiana

- Portato su Linux il motore Python con PDF diretti, verifica delle pagine, output atomico e controllo SHA-256 della sorgente.
- Aggiunta interfaccia GTK nativa con trascinamento, destinazione, avanzamento, annullamento e ripetizione degli errori.
- Conservato il flusso Bash/Zenity precedente tramite `--legacy`.

### English version

- Port the Python engine to Linux with direct PDFs, page verification, atomic output and source SHA-256 checks.
- Add native GTK file/folder drop, destination selection, progress, cancellation and retry.
- Retain the previous Bash/Zenity workflow through `--legacy`.

## [2.1.0] - 2026-10-06

### Versione italiana

- **Corretto:** nessuna cancellazione di cartelle preesistenti o rinomina degli archivi; riparazione RAR solo su copie private.
- **Corretto:** PDF pubblicati atomicamente dopo verifica di leggibilità e conteggio pagine; gli errori non lasciano PDF incompleti conteggiati come successi.
- **Corretto:** estensioni maiuscole, nomi con newline, pagine in sottocartelle e collisioni tra percorsi di output.
- **Corretto:** percorso assoluto del log, conservazione di stderr, codici di uscita e annullamento con arresto dei processi figli.
- **Aggiunto:** CLI senza interfaccia grafica, numero di job, percorso del log e qualità JPEG opzionale.
- **Modificato:** conversione senza perdita predefinita, un solo documento img2pdf, primo fotogramma per immagini animate/multipagina.
- **Sicurezza:** estrazione ZIP rifiuta percorsi non sicuri, collegamenti simbolici e membri duplicati.
- **Sviluppo:** build Debian da sorgente canonica, dipendenze esplicite e documentazione bilingue aggiornata.

### English version

- **Fixed:** existing directories and source archives preserved; RAR repair only on private copies.
- **Fixed:** atomic PDF publication after readability/page-count validation; failures cannot count incomplete PDFs as successful conversions.
- **Fixed:** uppercase extensions, newline filenames, nested pages and conflicting output paths.
- **Fixed:** absolute logfile paths, retained stderr, meaningful exit codes and cancellation of conversion children.
- **Added:** headless CLI, configurable jobs/logfile and optional JPEG quality.
- **Changed:** lossless conversion by default, one img2pdf document, first frame of animated/multipage images.
- **Security:** ZIP extraction rejects unsafe paths, symbolic links and duplicate members.
- **Development:** canonical-source Debian build, explicit dependencies and updated bilingual documentation.

## [2.0.0] - 2025-12-09

> [🇮🇹 Versione Italiana](#versione-italiana) | [🇬🇧 English Version](#english-version)

---

## Versione Italiana

Tutte le modifiche significative al progetto Advanced Comics Converter saranno documentate in questo file.

Il formato è basato su [Keep a Changelog](https://keepachangelog.com/it/1.0.0/),
e questo progetto aderisce al [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2025-12-10

### 🎉 Rilascio Maggiore - Prestazioni e Affidabilità

Questa versione introduce miglioramenti significativi nelle prestazioni e nell'affidabilità della conversione, con particolare focus sull'elaborazione parallela e sulla gestione intelligente degli errori.

### ✨ Aggiunte

- **Elaborazione parallela con GNU Parallel**
  - Supporto per elaborazione simultanea di più file (fino all'80% dei core CPU)
  - Riduzione drastica dei tempi di conversione per grandi collezioni
  - Gestione ottimizzata della memoria e delle risorse

- **Barra di progresso interattiva**
  - Visualizzazione real-time dello stato di conversione
  - Indicatore percentuale e conteggio file processati
  - Auto-chiusura al completamento

- **Validazione estensioni tramite Magic Bytes**
  - Rilevamento automatico del formato reale del file (ZIP vs RAR)
  - Correzione automatica delle estensioni errate (.cbz → .cbr o viceversa)
  - Supporto per RAR4 e RAR5

- **Sistema di riparazione automatica CBR**
  - Utilizzo di `rar r -y` per tentare la riparazione di archivi corrotti
  - Gestione trasparente dei file riparati
  - Fallback su file originale se la riparazione fallisce

- **Strategia di estrazione multi-tool**
  - Tentativo sequenziale con rar, unar, unrar, 7z
  - Verifica automatica del successo dell'estrazione
  - Logging dettagliato dei tentativi falliti

- **Skip intelligente dei file già convertiti**
  - Controllo preventivo dell'esistenza del PDF di output
  - Riduzione del carico di lavoro per esecuzioni multiple
  - Report accurato dei file skippati

### 🔄 Modifiche

- **Sistema di logging ottimizzato**
  - Riduzione drastica dell'output verboso
  - Focus su errori critici e statistiche finali
  - Formato log più leggibile e strutturato

- **Gestione migliorata dei file temporanei**
  - Pulizia automatica delle directory di lavoro
  - Rimozione dei metadati macOS (._*)
  - Gestione sicura dei file di progresso

- **Interfaccia utente Zenity potenziata**
  - Finestra di riepilogo finale con statistiche dettagliate
  - Visualizzazione emoji per risultati (✅ ⭐ ❌)
  - Lista dei file falliti nel messaggio finale
  - Dimensioni finestra ottimizzate per leggibilità

- **Normalizzazione nomi file immagini**
  - Sistema di rinominazione sequenziale ottimizzato
  - Prefisso temporaneo `temp_` per evitare conflitti
  - Numerazione finale a 3 cifre (001.jpg, 002.jpg, etc.)

### 🐛 Correzioni

- **Risolto**: Race condition nella rinominazione delle immagini
- **Risolto**: Perdita di immagini durante la conversione in alcuni casi edge
- **Risolto**: Crash con archivi CBR multi-volume
- **Risolto**: Problemi di encoding nei nomi file con caratteri speciali
- **Risolto**: Memory leak con collezioni molto grandi
- **Risolto**: Zenity progress bar che non raggiungeva il 100%
- **Risolto**: Errore di permessi su directory temporanee

### 🔧 Miglioramenti tecnici

- **Performance**
  - Riduzione del 60-70% dei tempi di conversione su collezioni medie (50+ file)
  - Utilizzo ottimizzato della CPU con carico bilanciato
  - Riduzione dell'I/O disco tramite gestione intelligente dei buffer

- **Affidabilità**
  - Test di integrità ZIP prima dell'estrazione
  - Verifica del conteggio immagini estratte da CBR
  - Validazione della creazione dei PDF individuali
  - Controllo finale del merge PDF

- **Manutenibilità**
  - Codice refactorizzato per maggiore leggibilità
  - Commenti migliorati nelle sezioni critiche
  - Separazione logica delle funzioni di elaborazione

### ⚠️ Breaking Changes

Nessun breaking change in questa versione. Lo script mantiene la compatibilità con le esecuzioni precedenti.

### 📝 Note di migrazione

Se stavi usando una versione precedente:

1. Installa GNU Parallel: `sudo apt install parallel`
2. Verifica che tutti gli strumenti RAR siano aggiornati
3. I log ora sono più concisi - controlla il nuovo formato

---

## [1.5.0] - 2025-11-15

### ✨ Aggiunte

- **Supporto per archivi RAR5**
  - Riconoscimento magic bytes RAR5
  - Compatibilità con archivi compressi con algoritmo RAR5

- **Test di integrità ZIP**
  - Validazione pre-estrazione per file CBZ
  - Skip automatico di file corrotti irrecuperabili

### 🔄 Modifiche

- Migliorata la gestione degli errori durante la conversione JPG
- Ottimizzato il processo di pulizia dei file temporanei

### 🐛 Correzioni

- **Risolto**: Errore con nomi file contenenti spazi
- **Risolto**: Conversione fallita per immagini WebP

---

## [1.0.0] - 2025-10-01

### 🎉 Rilascio Iniziale

Prima versione stabile dello script Advanced Comics Converter.

### ✨ Funzionalità

- Conversione base CBZ → PDF
- Conversione base CBR → PDF
- Interfaccia grafica Zenity
- Estrazione con unzip e unrar
- Conversione immagini tramite ImageMagick
- Creazione PDF con img2pdf
- Merge PDF con pdftk
- Log degli errori

### 🔧 Strumenti supportati

- unzip per CBZ
- unrar per CBR
- ImageMagick (convert)
- img2pdf
- pdftk

---

## [0.5.0] - 2025-09-15 (Beta)

### ✨ Aggiunte

- Prototipo iniziale del convertitore
- Supporto sperimentale per CBZ
- Script batch sequenziale

### ⚠️ Limitazioni note

- Nessun supporto per elaborazione parallela
- Log verboso con troppo output
- Nessun controllo integrità
- Performance limitate su collezioni grandi

---

## Tipologie di modifiche

- `✨ Aggiunte` per nuove funzionalità
- `🔄 Modifiche` per cambiamenti a funzionalità esistenti
- `🗑️ Rimozioni` per funzionalità deprecate o rimosse
- `🐛 Correzioni` per bug fix
- `🔒 Sicurezza` per vulnerabilità corrette
- `📝 Documentazione` per modifiche alla documentazione
- `🔧 Miglioramenti tecnici` per ottimizzazioni interne
- `⚠️ Breaking Changes` per modifiche incompatibili con versioni precedenti

---


## English Version

All significant changes to the Advanced Comics Converter project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2025-12-10

### 🎉 Major Release - Performance and Reliability

This version introduces significant improvements in conversion performance and reliability, with particular focus on parallel processing and intelligent error handling.

### ✨ Added

- **Parallel processing with GNU Parallel**
  - Support for simultaneous processing of multiple files (up to 80% of CPU cores)
  - Drastic reduction in conversion times for large collections
  - Optimized memory and resource management

- **Interactive progress bar**
  - Real-time visualization of conversion status
  - Percentage indicator and processed file count
  - Auto-close on completion

- **Extension validation via Magic Bytes**
  - Automatic detection of actual file format (ZIP vs RAR)
  - Automatic correction of wrong extensions (.cbz → .cbr or vice versa)
  - Support for RAR4 and RAR5

- **Automatic CBR repair system**
  - Use of `rar r -y` to attempt repairing corrupted archives
  - Transparent handling of repaired files
  - Fallback to original file if repair fails

- **Multi-tool extraction strategy**
  - Sequential attempt with rar, unar, unrar, 7z
  - Automatic verification of extraction success
  - Detailed logging of failed attempts

- **Smart skip of already converted files**
  - Preventive check for output PDF existence
  - Reduced workload for multiple executions
  - Accurate reporting of skipped files

### 🔄 Changed

- **Optimized logging system**
  - Drastic reduction of verbose output
  - Focus on critical errors and final statistics
  - More readable and structured log format

- **Improved temporary file management**
  - Automatic cleanup of working directories
  - Removal of macOS metadata (._*)
  - Safe handling of progress files

- **Enhanced Zenity user interface**
  - Final summary window with detailed statistics
  - Emoji visualization for results (✅ ⭐ ❌)
  - List of failed files in final message
  - Optimized window sizes for readability

- **Image filename normalization**
  - Optimized sequential renaming system
  - Temporary prefix `temp_` to avoid conflicts
  - Final 3-digit numbering (001.jpg, 002.jpg, etc.)

### 🐛 Fixed

- **Fixed**: Race condition in image renaming
- **Fixed**: Image loss during conversion in some edge cases
- **Fixed**: Crash with multi-volume CBR archives
- **Fixed**: Encoding issues in filenames with special characters
- **Fixed**: Memory leak with very large collections
- **Fixed**: Zenity progress bar not reaching 100%
- **Fixed**: Permission errors on temporary directories

### 🔧 Technical improvements

- **Performance**
  - 60-70% reduction in conversion times on medium collections (50+ files)
  - Optimized CPU usage with balanced load
  - Reduced disk I/O through intelligent buffer management

- **Reliability**
  - ZIP integrity test before extraction
  - Verification of extracted image count from CBR
  - Validation of individual PDF creation
  - Final check of PDF merge

- **Maintainability**
  - Refactored code for better readability
  - Improved comments in critical sections
  - Logical separation of processing functions

### ⚠️ Breaking Changes

No breaking changes in this version. The script maintains compatibility with previous executions.

### 📝 Migration notes

If you were using a previous version:

1. Install GNU Parallel: `sudo apt install parallel`
2. Verify that all RAR tools are updated
3. Logs are now more concise - check the new format

---

## [1.5.0] - 2025-11-15

### ✨ Added

- **Support for RAR5 archives**
  - RAR5 magic bytes recognition
  - Compatibility with archives compressed with RAR5 algorithm

- **ZIP integrity test**
  - Pre-extraction validation for CBZ files
  - Automatic skip of irreparable corrupted files

### 🔄 Changed

- Improved error handling during JPG conversion
- Optimized temporary file cleanup process

### 🐛 Fixed

- **Fixed**: Error with filenames containing spaces
- **Fixed**: Failed conversion for WebP images

---

## [1.0.0] - 2025-10-01

### 🎉 Initial Release

First stable version of the Advanced Comics Converter script.

### ✨ Features

- Basic CBZ → PDF conversion
- Basic CBR → PDF conversion
- Zenity graphical interface
- Extraction with unzip and unrar
- Image conversion via ImageMagick
- PDF creation with img2pdf
- PDF merge with pdftk
- Error logging

### 🔧 Supported tools

- unzip for CBZ
- unrar for CBR
- ImageMagick (convert)
- img2pdf
- pdftk

---

## [0.5.0] - 2025-09-15 (Beta)

### ✨ Added

- Initial converter prototype
- Experimental support for CBZ
- Sequential batch script

### ⚠️ Known limitations

- No parallel processing support
- Verbose logging with too much output
- No integrity checking
- Limited performance on large collections

---

## Types of changes

- `✨ Added` for new features
- `🔄 Changed` for changes to existing features
- `🗑️ Removed` for deprecated or removed features
- `🐛 Fixed` for bug fixes
- `🔒 Security` for fixed vulnerabilities
- `📝 Documentation` for documentation changes
- `🔧 Technical improvements` for internal optimizations
- `⚠️ Breaking Changes` for changes incompatible with previous versions

---



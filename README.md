# Advanced Comics Converter

> [🇮🇹 Versione Italiana](#versione-italiana) | [🇬🇧 English Version](#english-version)

---





https://youtu.be/gxKtQuFBChE?si=MCsYmdSQaBraBKq0

## Versione Italiana

Script bash avanzato per la conversione automatica di file CBZ/CBR in formato PDF con interfaccia grafica Zenity.

## 📋 Caratteristiche

- **Conversione automatica**: Trasforma tutti i file CBZ/CBR in PDF mantenendo la qualità delle immagini
- **Interfaccia grafica**: Utilizza Zenity per una selezione intuitiva delle directory e feedback visivo
- **Elaborazione parallela**: Sfrutta GNU Parallel per processare più file contemporaneamente (fino all'80% dei core disponibili)
- **Barra di progresso**: Visualizzazione in tempo reale dello stato di conversione
- **Validazione estensioni**: Controlla e corregge automaticamente le estensioni errate basandosi sui magic bytes
- **Test di integrità**: Verifica l'integrità dei file CBZ prima dell'estrazione
- **Supporto multi-formato RAR**: Tenta l'estrazione con rar, unar, unrar e 7z in sequenza
- **Riparazione automatica**: Utilizza `rar r` per tentare di riparare archivi CBR corrotti
- **Skip intelligente**: Salta automaticamente i file già convertiti
- **Logging selettivo**: Registra solo gli errori critici per facilitare il debugging
- **Pulizia automatica**: Rimuove file temporanei e metadati macOS

## 🔧 Requisiti

### Dipendenze obbligatorie

```bash
sudo apt install imagemagick img2pdf pdftk unzip zenity coreutils parallel
```

### Dipendenze ALTAMENTE CONSIGLIATA

installare Winrar per Linux https://www.win-rar.com/rar-linux-mac.html

wget https://www.win-rar.com/fileadmin/winrar-versions/rarlinux-x64-712.tar.gz

tar -xvf rarlinux-x64-712.tar.gz

cd rar/

sudo cp rar unrar /usr/local/bin

### Strumenti per CBR (almeno uno richiesto)

```bash
# Opzione 1: rar (consigliato per riparazione)
sudo apt install rar

# Opzione 2: unar
sudo apt install unar

# Opzione 3: unrar
sudo apt install unrar

# Opzione 4: p7zip
sudo apt install p7zip-full
```

## 🚀 Utilizzo

### Modalità base

1. Rendi eseguibile lo script:
```bash
chmod +x advanced-comics-converter.sh
```

2. Esegui lo script:
```bash
./advanced-comics-converter.sh
```

3. Seleziona la directory contenente i file CBZ/CBR dalla finestra di dialogo

4. Attendi il completamento della conversione visualizzando la barra di progresso

### Output

- I file PDF vengono creati nella stessa directory dei file originali
- Nome file: `[nome_originale].pdf`
- Log errori: `comics_converter_errors.log` nella directory corrente


oppure il pacchetto deb

    sudo dpkg -i advanced-comics-converter_2.0.0_all.deb



## 📊 Funzionamento

### Processo di conversione

1. **Selezione directory**: Interfaccia Zenity per la scelta della cartella
2. **Verifica dipendenze**: Controllo automatico di tutti gli strumenti richiesti
3. **Scansione file**: Ricerca ricorsiva di tutti i file .cbz e .cbr
4. **Validazione estensioni**: Controllo dei magic bytes e correzione se necessario
5. **Filtro skip**: Esclusione dei file già convertiti (PDF esistente)
6. **Estrazione parallela**: Elaborazione simultanea con GNU Parallel
7. **Conversione immagini**: Normalizzazione in formato JPG
8. **Creazione PDF**: Generazione di PDF individuali per pagina
9. **Merge finale**: Unione di tutte le pagine in un unico PDF
10. **Pulizia**: Rimozione di file temporanei e directory di lavoro

### Magic Bytes supportati

- **ZIP (CBZ)**: `50 4B 03 04`, `50 4B 05 06`, `50 4B 07 08`
- **RAR4 (CBR)**: `52 61 72 21`
- **RAR5 (CBR)**: `06 00 00 00 52 61 72 21 1A 07 01 00`

### Strategia di estrazione CBR

Lo script tenta l'estrazione in questo ordine:

1. **rar** con riparazione (`rar r -y`) - *consigliato*
2. **unar** - estrazione e spostamento immagini
3. **unrar** - estrazione diretta
4. **7z** - fallback finale

## 📝 Log e Debugging

Il file `comics_converter_errors.log` contiene:

- Timestamp di esecuzione
- Directory selezionata
- Errori di estrazione
- Errori di conversione
- Errori di merge PDF
- Statistiche finali

Esempio di log:
```
====================================================================
Log Errori - Mar 10 Dic 2024 15:30:45 CET
====================================================================
Directory selezionata: /home/user/fumetti
ERROR: ZIP extraction failed for /home/user/fumetti/corrupted.cbz
SUCCESS: /home/user/fumetti/spiderman_001.cbz converted 24 pages
====================================================================
Summary:
Converted files: 15
Skipped files: 3
Failed files: 1
====================================================================
```

## 🎯 Casi d'uso

### Convertire un'intera collezione

```bash
./advanced-comics-converter.sh
# Seleziona la directory principale della collezione
# Lo script processerà ricorsivamente tutte le sottodirectory
```

### Riconvertire file falliti

Lo script skippa automaticamente i file già convertiti. Per riconvertire:

1. Elimina i PDF falliti
2. Riesegui lo script sulla stessa directory

### Gestione file corrotti

Lo script tenta automaticamente la riparazione dei CBR con `rar r`. Se fallisce:

1. Controlla il log per identificare il file
2. Prova manualmente: `rar r -y file.cbr`
3. Riconverti il file riparato

## ⚠️ Limitazioni

- **Formati supportati**: Solo CBZ (ZIP) e CBR (RAR/RAR5)
- **Immagini**: Conversione forzata a JPG per compatibilità
- **Memoria**: L'elaborazione parallela richiede RAM proporzionale al numero di core
- **Spazio disco**: Richiede spazio temporaneo pari a ~2x la dimensione del file più grande

## 🔍 Risoluzione problemi

### "Command not found"

Installa le dipendenze mancanti:
```bash
sudo apt install [comando-mancante]
```

### "No RAR tool found"

Installa almeno uno strumento RAR:
```bash
sudo apt install rar
```

### "Cannot access selected directory"

Verifica i permessi della directory:
```bash
chmod +rx /path/to/directory
```

### File CBR non estratti

1. Verifica quale strumento RAR è installato
2. Testa manualmente l'estrazione:
```bash
rar t file.cbr  # test integrità
unar -t file.cbr  # test con unar
```

### Zenity non si apre

Verifica che X11 sia disponibile:
```bash
echo $DISPLAY
```

### Conversione troppo lenta

Riduci il parallelismo modificando lo script:
```bash
parallel -j 50%  # invece di 80%
```

## 🛠️ Personalizzazione

### Modificare il livello di parallelismo

Linea 338:
```bash
printf "%s\n" "${FILES_TO_PROCESS[@]}" | parallel -j 80% '
```

Cambia `80%` con:
- `50%` per sistemi con poca RAM
- `100%` per sistemi potenti
- `4` per un numero fisso di job

### Modificare la qualità JPG

Linea 180:
```bash
convert "$img" -quality 85 "$TEMPNAME" 2>/dev/null
```

Aggiungi `-quality [1-100]` per controllare la compressione

### Abilitare log verboso

Rimuovi tutti i `2>/dev/null` e `>/dev/null 2>&1` per vedere l'output completo

## 📜 Licenza

Script rilasciato come software libero. Utilizzabile, modificabile e distribuibile liberamente.

## 👤 Autore

Franco Conidi aka Edmond - SysLinuxOS
System Integrator, Network Engineer, IT Consultant Blogger Linux Developer https://francoconidi.it https://syslinuxos.com

## 🔗 Link utili

- [ImageMagick Documentation](https://imagemagick.org/index.php)
- [img2pdf Documentation](https://gitlab.mister-muffin.de/josch/img2pdf)
- [PDFtk Documentation](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/)
- [GNU Parallel Tutorial](https://www.gnu.org/software/parallel/parallel_tutorial.html)

## 📅 Versione

Versione attuale: 2.0.0 (vedi CHANGELOG.md per dettagli)

---

**Note**: Questo script è ottimizzato per sistemi Linux/Unix. Per Windows, considera l'uso di WSL (Windows Subsystem for Linux).

---

## English Version

Advanced bash script for automatic conversion of CBZ/CBR files to PDF format with Zenity graphical interface.

## 📋 Features

- **Automatic conversion**: Transforms all CBZ/CBR files to PDF while maintaining image quality
- **Graphical interface**: Uses Zenity for intuitive directory selection and visual feedback
- **Parallel processing**: Leverages GNU Parallel to process multiple files simultaneously (up to 80% of available cores)
- **Progress bar**: Real-time visualization of conversion status
- **Extension validation**: Automatically checks and corrects wrong extensions based on magic bytes
- **Integrity testing**: Verifies CBZ file integrity before extraction
- **Multi-format RAR support**: Attempts extraction with rar, unar, unrar, and 7z sequentially
- **Automatic repair**: Uses `rar r` to attempt repairing corrupted CBR archives
- **Smart skip**: Automatically skips already converted files
- **Selective logging**: Records only critical errors for easier debugging
- **Automatic cleanup**: Removes temporary files and macOS metadata

## 🔧 Requirements

### Mandatory dependencies

```bash
sudo apt install imagemagick img2pdf pdftk unzip zenity coreutils parallel
```

Install Winrar for Linux https://www.win-rar.com/rar-linux-mac.html

wget https://www.win-rar.com/fileadmin/winrar-versions/rarlinux-x64-712.tar.gz

tar -xvf rarlinux-x64-712.tar.gz

cd rar/

sudo cp rar unrar /usr/local/bin

### CBR tools (at least one required)

```bash
# Option 1: rar (recommended for repair)
sudo apt install rar

# Option 2: unar
sudo apt install unar

# Option 3: unrar
sudo apt install unrar

# Option 4: p7zip
sudo apt install p7zip-full
```

## 🚀 Usage

### Basic mode

1. Make the script executable:
```bash
chmod +x advanced-comics-converter.sh
```

2. Run the script:
```bash
./advanced-comics-converter.sh
```

3. Select the directory containing CBZ/CBR files from the dialog window

4. Wait for conversion to complete while viewing the progress bar

Or using deb package

    sudo dpkg -i advanced-comics-converter_2.0.0_all.deb



### Output

- PDF files are created in the same directory as the original files
- File name: `[original_name].pdf`
- Error log: `comics_converter_errors.log` in the current directory

## 📊 How it works

### Conversion process

1. **Directory selection**: Zenity interface for folder selection
2. **Dependencies check**: Automatic verification of all required tools
3. **File scanning**: Recursive search for all .cbz and .cbr files
4. **Extension validation**: Magic bytes checking and correction if necessary
5. **Skip filter**: Exclusion of already converted files (existing PDF)
6. **Parallel extraction**: Simultaneous processing with GNU Parallel
7. **Image conversion**: Normalization to JPG format
8. **PDF creation**: Generation of individual PDFs per page
9. **Final merge**: Merging all pages into a single PDF
10. **Cleanup**: Removal of temporary files and working directories

### Supported Magic Bytes

- **ZIP (CBZ)**: `50 4B 03 04`, `50 4B 05 06`, `50 4B 07 08`
- **RAR4 (CBR)**: `52 61 72 21`
- **RAR5 (CBR)**: `06 00 00 00 52 61 72 21 1A 07 01 00`

### CBR extraction strategy

The script attempts extraction in this order:

1. **rar** with repair (`rar r -y`) - *recommended*
2. **unar** - extraction and image moving
3. **unrar** - direct extraction
4. **7z** - final fallback

## 📝 Logging and Debugging

The `comics_converter_errors.log` file contains:

- Execution timestamp
- Selected directory
- Extraction errors
- Conversion errors
- PDF merge errors
- Final statistics

Example log:
```
====================================================================
Log Errori - Tue Dec 10 15:30:45 CET 2024
====================================================================
Directory selezionata: /home/user/comics
ERROR: ZIP extraction failed for /home/user/comics/corrupted.cbz
SUCCESS: /home/user/comics/spiderman_001.cbz converted 24 pages
====================================================================
Summary:
Converted files: 15
Skipped files: 3
Failed files: 1
====================================================================
```

## 🎯 Use cases

### Convert an entire collection

```bash
./advanced-comics-converter.sh
# Select the main collection directory
# The script will recursively process all subdirectories
```

### Reconvert failed files

The script automatically skips already converted files. To reconvert:

1. Delete failed PDFs
2. Re-run the script on the same directory

### Handling corrupted files

The script automatically attempts CBR repair with `rar r`. If it fails:

1. Check the log to identify the file
2. Try manually: `rar r -y file.cbr`
3. Reconvert the repaired file

## ⚠️ Limitations

- **Supported formats**: Only CBZ (ZIP) and CBR (RAR/RAR5)
- **Images**: Forced conversion to JPG for compatibility
- **Memory**: Parallel processing requires RAM proportional to the number of cores
- **Disk space**: Requires temporary space equal to ~2x the size of the largest file

## 🔍 Troubleshooting

### "Command not found"

Install missing dependencies:
```bash
sudo apt install [missing-command]
```

### "No RAR tool found"

Install at least one RAR tool:
```bash
sudo apt install rar
```

### "Cannot access selected directory"

Check directory permissions:
```bash
chmod +rx /path/to/directory
```

### CBR files not extracted

1. Check which RAR tool is installed
2. Test extraction manually:
```bash
rar t file.cbr  # integrity test
unar -t file.cbr  # test with unar
```

### Zenity doesn't open

Verify X11 is available:
```bash
echo $DISPLAY
```

### Conversion too slow

Reduce parallelism by modifying the script:
```bash
parallel -j 50%  # instead of 80%
```

## 🛠️ Customization

### Modify parallelism level

Line 338:
```bash
printf "%s\n" "${FILES_TO_PROCESS[@]}" | parallel -j 80% '
```

Change `80%` to:
- `50%` for systems with low RAM
- `100%` for powerful systems
- `4` for a fixed number of jobs

### Modify JPG quality

Line 180:
```bash
convert "$img" -quality 85 "$TEMPNAME" 2>/dev/null
```

Add `-quality [1-100]` to control compression

### Enable verbose logging

Remove all `2>/dev/null` and `>/dev/null 2>&1` to see complete output

## 📜 License

Script released as free software. Freely usable, modifiable, and distributable.

## 👤 Author

Franco Conidi aka Edmond - SysLinuxOS
System Integrator, Network Engineer, IT Consultant Blogger Linux Developer https://francoconidi.it https://syslinuxos.com

## 🔗 Useful links

- [ImageMagick Documentation](https://imagemagick.org/index.php)
- [img2pdf Documentation](https://gitlab.mister-muffin.de/josch/img2pdf)
- [PDFtk Documentation](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/)
- [GNU Parallel Tutorial](https://www.gnu.org/software/parallel/parallel_tutorial.html)

## 📅 Version

Current version: 2.0.0 (see CHANGELOG.md for details)

---

**Note**: This script is optimized for Linux/Unix systems. For Windows, consider using WSL (Windows Subsystem for Linux).

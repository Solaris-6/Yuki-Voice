[README.md](https://github.com/user-attachments/files/27982437/README.md)
# Yuki Voice Integration for Home Assistant

Integrazione Home Assistant per dare voce al sistema Yuki AI attraverso dispositivi audio di rete e locali.

## Funzionalità

- **Scansione rete**: Scansiona automaticamente la rete locale per identificare dispositivi audio compatibili (Chromecast, Sonos, speaker generici)
- **Dispositivi locali**: Rileva automaticamente dispositivi audio locali collegati al sistema
- **Text-to-Speech Yuki**: Utilizza il sistema vocale Yuki per sintetizzare messaggi
- **Servizi personalizzati**: Tre servizi per interagire con il sistema audio

## Installazione

### 1. Copia i file dell'integrazione

Copia l'intera cartella `yuki_voice` nella directory `custom_components` di Home Assistant:

```bash
\\192.168.1.115\config\custom_components\yuki_voice\
```

### 2. Riavvia Home Assistant

Riavvia Home Assistant per caricare la nuova integrazione.

### 3. Configura l'integrazione

1. Vai in **Impostazioni** > **Dispositivi e servizi**
2. Clicca su **+ Aggiungi integrazione**
3. Cerca "Yuki Voice"
4. Configura:
   - **Scan Network**: Scansiona la rete per dispositivi audio (default: true)
   - **Auto Detect Audio**: Rileva automaticamente dispositivi locali (default: true)
   - **Yuki Voice Path**: Percorso del sistema Yuki (default: F:/IA)

## Servizi

### speak

Sintetizza e riproduce un messaggio vocale.

```yaml
service: yuki_voice.speak
data:
  message: "Ciao, sono Yuki"
  language: "it"
  device: "Yuki Local Speakers"
```

**Parametri:**
- `message` (richiesto): Il messaggio da riprodurre
- `language` (opzionale): Codice lingua (it, en) - default: it
- `device` (opzionale): Dispositivo audio target

### scan_audio_devices

Scansiona la rete per dispositivi audio.

```yaml
service: yuki_voice.scan_audio_devices
data:
  subnet: "192.168.1.0/24"
```

**Parametri:**
- `subnet` (opzionale): Subnet da scansionare - default: 192.168.1.0/24

### play_yuki_audio

Riproduce un file audio dalla cartella Yuki.

```yaml
service: yuki_voice.play_yuki_audio
data:
  filename: "Ciao io sono Yuki e .wav"
  device: "Yuki Local Speakers"
```

**Parametri:**
- `filename` (richiesto): Nome del file audio in F:/IA/audio
- `device` (opzionale): Dispositivo audio target

## Dispositivi Supportati

### Dispositivi di Rete
- **Chromecast**: Google Cast devices
- **Sonos**: Sonos speakers
- **Generic Audio**: Qualsiasi dispositivo con server HTTP audio

### Dispositivi Locali
- Tutti i dispositivi audio rilevati da PortAudio (sounddevice)

## Struttura File

```
F:\Test Integrazione\
├── __init__.py              # Setup principale integrazione
├── manifest.json            # Manifesto integrazione HA
├── config_flow.py           # Flusso di configurazione
├── const.py                 # Costanti
├── network_scanner.py       # Scanner dispositivi rete
├── media_player.py         # Platform media player
├── tts.py                   # Platform text-to-speech
├── services.py              # Servizi personalizzati
├── services.yaml            # Definizione servizi
├── strings.json             # Traduzioni
├── requirements.txt         # Dipendenze Python
└── README.md                # Documentazione
```

## Configurazione Avanzata

### Percorso Yuki

Il percorso predefinito è `F:/IA`, ma può essere configurato durante l'installazione o modificato nel file `config_entry`.

### Scansione Rete

La scansione rete controlla le porte comuni audio:
- 80, 443 (HTTP/HTTPS)
- 8080 (HTTP alternativo)
- 49152 (UPnP)
- 5000, 8000, 9000 (Servizi audio comuni)

## Troubleshooting

### Dispositivi non rilevati

1. Verifica che i dispositivi siano sulla stessa rete
2. Controlla il firewall per permettere la scansione porte
3. Usa il servizio `scan_audio_devices` con il subnet corretto

### Audio non riprodotto

1. Verifica che il dispositivo audio sia selezionato
2. Controlla i log di Home Assistant per errori
3. Assicurati che le dipendenze siano installate

### Sistema Yuki non disponibile

Se il sistema Yuki non è disponibile, l'integrazione userà il TTS di sistema (pyttsx3) come fallback.

## Dipendenze

- aiohttp>=3.8.0
- pyaudio>=0.2.11
- sounddevice>=0.4.6
- soundfile>=0.12.0
- pyttsx3>=2.90

## Note

- L'integrazione richiede Home Assistant 2023.1 o superiore
- I dispositivi audio devono essere accessibili dalla rete locale
- Il sistema Yuki deve essere accessibile al percorso configurato

## Supporto

Per problemi o suggerimenti, contatta Tony o apri una issue sul repository GitHub.

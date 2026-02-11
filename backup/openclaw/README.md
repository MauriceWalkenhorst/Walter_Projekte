# OpenClaw Backup - Walter KI

**Erstellt:** 2026-02-11  
**Enthält:** Memory + Konfiguration (ohne Secrets & Sessions)

## ⚠️ WICHTIG: Secrets separat sichern!

Dieses Backup enthält **keine API-Keys** und **keine Chat-Sessions** (wegen GitHub Secret Scanning).

### Manuelle Wiederherstellung der Secrets:

**1. auth-profiles.json** ergänzen:
```json
{
  "profiles": {
    "google:default": {
      "type": "api_key",
      "provider": "google",
      "key": "DEIN_GOOGLE_API_KEY"
    },
    "openrouter:default": {
      "type": "api_key", 
      "provider": "openrouter",
      "key": "DEIN_OPENROUTER_KEY"
    }
  }
}
```

**2. openclaw.json** - Telegram Token einfügen:
```json
"telegram": {
  "botToken": "DEIN_TELEGRAM_BOT_TOKEN"
}
```

## Enthaltene Daten

| Datei/Ordner | Inhalt | Wichtigkeit |
|--------------|--------|-------------|
| `main.sqlite` | Memory-Datenbank (Walter's Langzeitgedächtnis) | ⭐⭐⭐ Kritisch |
| `openclaw.json` | Konfiguration (Modelle, Gateway, Skills) | ⭐⭐⭐ Kritisch |
| `auth-profiles.json` | API-Key Struktur (Keys = PLACEHOLDER) | ⭐⭐⭐ Kritisch |
| `canvas/` | Hochgeladene Bilder/Dateien | ⭐⭐ Wichtig |
| `cron/` | Automatisierte Aufgaben | ⭐ Mittel |

## NICHT enthalten (zu groß/Secrets):
- `sessions/` - Chat-Verlauf (300+ Dateien, 17MB)
- `credentials/` - Skill API-Keys
- `telegram/` - Telegram Einstellungen

## Wiederherstellung

### 1. Sessions separat backuppen (lokal):
```bash
# Auf dem Server:
tar czf walter_sessions_$(date +%Y%m%d).tar.gz /docker/openclaw-3pf3/config/agents/main/sessions/
```

### 2. OpenClaw neu installieren

### 3. Dieses Backup einspielen:
```bash
cp backup/openclaw/* /docker/openclaw-3pf3/config/
# Secrets einfügen!
```

### 4. Sessions zurückspielen:
```bash
tar xzf walter_sessions_20260211.tar.gz -C /docker/openclaw-3pf3/config/agents/main/
```

### 5. Rechte fixen:
```bash
chown -R ubuntu:ubuntu /docker/openclaw-3pf3/config
chmod 700 /docker/openclaw-3pf3/config/credentials
chmod 600 /docker/openclaw-3pf3/config/openclaw.json
```

## Komplettes Backup (lokal auf Server)

Für vollständiges Backup mit Sessions & Secrets:
```bash
tar czf walter_ki_komplett_$(date +%Y%m%d).tar.gz /docker/openclaw-3pf3/config/
```

# Wirtschafts-Briefing

Erstellt dienstags ein Wirtschafts-Briefing aus aktuellen RSS-Nachrichten und Marktdaten. Claude filtert und fasst die Meldungen zusammen; das Ergebnis wird als PDF gespeichert und optional als HTML-E-Mail versendet.

## Funktionen

- Nachrichten aus sechs Kategorien: Allgemein, Finanzen, Krypto, Forex, Tech und Rohstoffe
- Marktdaten für Indizes, Kryptowährungen, Rohstoffe und EUR/USD
- Auswahl und Zusammenfassung der wichtigsten Meldungen mit Claude
- PDF-Erstellung und E-Mail-Versand über Gmail SMTP
- Automatischer Start über GitHub Actions

## Einrichtung

Voraussetzungen sind Python 3.11, ein Anthropic API-Key und für den E-Mail-Versand ein Gmail-App-Passwort.

```bash
git clone git@github.com:Benchmark-Bene/test.git
cd test
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Lege im Projektverzeichnis eine nicht versionierte `.env` an:

```env
ANTHROPIC_API_KEY=dein_api_key
EMAIL_PASSWORD=dein_gmail_app_passwort
```

Passe anschließend in `config.yaml` mindestens Absender und Empfänger an. Dort lassen sich außerdem RSS-Feeds, Marktsymbole, Nachrichtenanzahl, Zeitfilter und Ausgabeoptionen konfigurieren.

## Ausführen

```bash
python main.py
```

Das Programm beendet sich ohne Verarbeitung, wenn es nicht an einem Dienstag gestartet wird. PDF-Erstellung und E-Mail-Versand werden in `config.yaml` über `output.generate_pdf` und `output.send_email` gesteuert.

## GitHub Actions

Der vorhandene Workflow `.github/workflows/daily-briefing.yml` startet täglich um 18:00 UTC; `main.py` erstellt das Briefing jedoch nur dienstags. Hinterlege unter **Settings → Secrets and variables → Actions** diese Repository-Secrets:

- `ANTHROPIC_API_KEY`
- `EMAIL_PASSWORD`

Der Workflow kann unter **Actions → Daily Economic Briefing → Run workflow** auch manuell gestartet werden. Wegen der Dienstagsprüfung erzeugt ein manueller Lauf an anderen Wochentagen kein Briefing.

## Wichtige Dateien

| Datei | Zweck |
|---|---|
| `config.yaml` | E-Mail-, Feed-, Markt- und Ausgabe-Einstellungen |
| `main.py` | Ablauf und Dienstagsprüfung |
| `modules/` | Abruf, Filterung, Zusammenfassung und Ausgabe |
| `templates/email_template.html` | Layout der HTML-E-Mail |
| `.github/workflows/daily-briefing.yml` | Automatischer Workflow |

Bei Fehlern zuerst die Konsolenausgabe beziehungsweise das GitHub-Actions-Log prüfen. Das Programm muss aus dem Repository-Hauptverzeichnis gestartet werden.

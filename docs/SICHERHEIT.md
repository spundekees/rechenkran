# Sicherheitsprüfung (normale Version)

Stand: 26.11.2026 · geprüft: `index.html`

| Prüfung | Werkzeug | Ergebnis |
|---|---|---|
| Secret-Scan der Git-Historie | Gitleaks 8.21.2 (MIT) | ✅ keine Funde (7 Commits) |
| Lint + SAST | ESLint 10.15.0 mit `eslint-plugin-security`, `eslint-plugin-no-unsanitized` (Konfiguration aus `rechenkranlw`) | ⚠️ 23 Funde: 14× `innerHTML` mit zusammengesetzten Werten, 6× unbenutzte Variablen, 2× leere `catch`-Blöcke, 1× `moveTo` verdeckt eine Browser-Funktion |
| SAST mit eigenen Regeln | Semgrep 1.187.0, Regeln aus `rechenkranlw/.semgrep/rules.yml` | ⚠️ 14 Funde, alle Regel `html-einfuegen-mit-variablen` |
| Abhängigkeiten | – | keine Abhängigkeiten |

## Bewertung

Die einzige Eingabe von außen sind die gespeicherten Einstellungen im `localStorage` des eigenen Geräts.
Diese werden **ungeprüft** übernommen. Wer Zugriff auf das Gerät bzw. die Browser-Werkzeuge hat, könnte dort
HTML hinterlegen, das beim nächsten Start ausgeführt wird (Self-XSS). Risiko: **niedrig**, da kein Fremdzugriff
über das Netz möglich ist und keine Daten anderer Personen verarbeitet werden.

## Offene Punkte (nicht umgesetzt, Entscheidung offen)

Die Version `rechenkranlw` enthält die Behebungen bereits: Eingabeprüfung (`sanitizeSettings`), HTML nur aus festen
Bausteinen, Logging-Modul, Tests. Sie können in diese Version übernommen werden.

## Datenschutz

Aufruf über GitHub Pages: Der Browser übermittelt technisch die IP-Adresse an GitHub (USA).
Gespeichert werden nur Einstellungen im `localStorage` des Geräts, keine personenbezogenen Daten.
Details: `rechenkranlw/docs/DATENSCHUTZ.md`.

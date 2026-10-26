# Projektprofil – code-sniffer

Stand: 25.10.2026 · Repository: `dasann/code-sniffer` · Basisbranch: `master`

## Geprüfte Ausgangslage

Spryker/Glue CodeSniffer-Regelsammlung, PHP >=8.1, PHPStan Level 8. Die Konfiguration wurde aus dem aktuellen Repository gelesen. Das ist kein Nachweis des produktiven Betriebs.

## Einführung und Geltungsbereich

Die gemeinsamen Regeln gelten für neue und geänderte eigene Dateien. Der neue CI-Job **Coding standards / changed-files** prüft vollständige geänderte Dateien im PR-Diff; er formatiert keinen Altbestand automatisch. Eine Änderung an einer alten Datei kann deshalb zunächst deren vorhandene Stilbefunde sichtbar machen. Es wird keine pauschale Fehler-Baseline angelegt.

Vorhandene Anwendungsprüfungen bleiben zusätzlich erforderlich. Die Einführung ist keine vollständige Strict-/Framework-Migration und keine fachliche Abnahme. Einstellungen der GitHub-Merge-Regeln und automatische Codex-Reviews sind außerhalb dieses Datei-PRs zu aktivieren beziehungsweise zu verifizieren.

## Projektbezogene Präzisierungen und offene Punkte

- SprykerStrict und Glue-spezifische Regeln bleiben maßgeblich zusätzlich zu PSR-12. Keine Sniffs, Schweregrade oder Test-Fixtures pauschal umschreiben. PHPCS 4 läuft isoliert für den neuen PSR-12-Check; die vorhandene Paketabhängigkeit auf PHPCS 3 bleibt unverändert.
- Bestehende CI verwendet ubuntu-20.04 und ältere Actions. Aktualisierung dieser bestehenden Pipeline ist ein offener Betriebsbefund, kein bestandener Check dieses PRs.

## Prüfkommandos

```sh
npm ci --prefix .quality --ignore-scripts
# BASE_SHA durch den konkreten Vergleichscommit ersetzen:
node .quality/check.mjs --base BASE_SHA
# Gezielt alle getrackten Dateien prüfen (kann Altbefunde zeigen):
node .quality/check.mjs --all
```

Der Vergleichscommit muss als echter SHA übergeben werden. `--base` prüft den eingecheckten Diff bis HEAD; uncommittete Änderungen bei Bedarf explizit mit `--files pfad/zur/datei` prüfen. Der neue Job benötigt Node 24.19 oder neuer innerhalb der 24er-Reihe; die Produktionslaufzeit bleibt unverändert.

Für PHP-Dateien ist zusätzlich PHP_CodeSniffer 4.0.4 nötig (`phpcs` im PATH oder `PHPCS_BIN`). CI installiert ihn isoliert. Warnungen bleiben sichtbar; PSR-12-Fehler blockieren.

Vorhandene ergänzende Kommandos:

- `composer cs-check` — vorhanden; passende isolierte Testumgebung erforderlich.
- `composer test` — vorhanden; passende isolierte Testumgebung erforderlich.
- `composer stan` — vorhanden; passende isolierte Testumgebung erforderlich.
- `composer xml` — vorhanden; passende isolierte Testumgebung erforderlich.

Vorhandene Workflows: `.github/workflows/ci.yml`.

## Nachweis und Freigabe

Der Standards-Job liefert Belege für Anweisungsdateien, Format und anwendbare Sprachregeln im geänderten Umfang. Er ersetzt weder vollständige Typechecks/Builds noch fachliche Tests, Browserprüfungen, Kotlin-Tests oder Sicherheitsabnahmen. Fehlende Nachweise als „nicht geprüft“ ausweisen.

Codex folgt `docs/REVIEW_CHECKLIST.md`. Freigabe von Merge, Release und Livegang liegt bei David Sann. Neue Ausnahmen benötigen Regel-ID, engen Umfang, Begründung und echte Freigabereferenz; diese Einführung erfindet keine pauschal genehmigten Ausnahmen.

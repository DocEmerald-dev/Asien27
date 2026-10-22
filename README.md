# Asien-Reiseplaner

Gemeinsame Reiseplanung für eure Asienreise: Jede Person meldet sich mit eigenem Konto an, bewertet Ziele und trägt ein, wann sie Zeit hat. Die Seite berechnet daraus automatisch, wer wann zusammen wohin fährt.

Die Seite selbst liegt kostenlos auf **GitHub Pages**. Login und Datenbank laufen über **Firebase** (kostenloser Google-Dienst, für eine Freundesgruppe reicht der Gratis-Tarif „Spark“ dauerhaft). Einmalige Einrichtung: etwa 15 Minuten.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Die Website |
| `config.js` | Zugangsdaten deines Firebase-Projekts (musst du ausfüllen) |
| `firestore.rules` | Regeln, wer was in der Datenbank lesen und ändern darf |
| `README.md` | Diese Anleitung |

## Schritt 1: Firebase-Projekt anlegen

1. Öffne <https://console.firebase.google.com> und melde dich mit einem Google-Konto an.
2. „Projekt erstellen“ → Namen eingeben (z. B. `asienreise`) → Google Analytics kannst du ausschalten → „Projekt erstellen“.

## Schritt 2: Login per E-Mail einschalten

1. Links im Menü: **Build → Authentication** → „Jetzt starten“.
2. Reiter **Anmeldemethode** → **E-Mail/Passwort** → den ersten Schalter aktivieren → „Speichern“.

## Schritt 3: Datenbank anlegen und Regeln setzen

1. Links: **Build → Firestore Database** → „Datenbank erstellen“.
2. Standort z. B. `europe-west3 (Frankfurt)` wählen, dann **Produktionsmodus** → „Erstellen“.
3. Reiter **Regeln** öffnen, den gesamten Inhalt löschen, den Inhalt von `firestore.rules` einfügen → „Veröffentlichen“.

**Empfehlung:** Ohne Freigabeliste kann jeder, der den Link kennt, sich registrieren und mitplanen. Wenn nur ihr planen sollt, folge den Hinweisen oben in `firestore.rules` (Variante B), trage eure E-Mail-Adressen ein und veröffentliche die Regeln erneut.

## Schritt 4: `config.js` ausfüllen

1. Oben links auf das **Zahnrad → Projekteinstellungen** → Reiter „Allgemein“.
2. Unten bei „Meine Apps“ auf das Web-Symbol **`</>`** klicken → App-Namen eingeben → „App registrieren“ (Firebase Hosting **nicht** ankreuzen).
3. Es erscheint ein Code-Block mit `const firebaseConfig = { ... }`. Kopiere die sechs Werte (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`) in die entsprechenden Zeilen von `config.js`.

Diese Werte sind nicht geheim und dürfen öffentlich auf GitHub stehen. Geschützt wird die Reise durch die Regeln aus Schritt 3.

## Schritt 5: Auf GitHub veröffentlichen

1. Auf <https://github.com> ein neues Repository anlegen (z. B. `asienreise`), Sichtbarkeit **Public** (GitHub Pages ist im kostenlosen Konto nur für öffentliche Repositories verfügbar).
2. „uploading an existing file“ → `index.html`, `config.js`, `firestore.rules` und `README.md` hineinziehen → „Commit changes“.
3. Im Repository: **Settings → Pages** → bei „Source“ **Deploy from a branch** → Branch `main`, Ordner `/ (root)` → „Save“.
4. Nach 1–2 Minuten ist die Seite erreichbar unter `https://DEIN-BENUTZERNAME.github.io/asienreise/` (der Link steht auch oben auf der Pages-Seite).

## Schritt 6: GitHub-Adresse in Firebase eintragen

In Firebase: **Authentication → Einstellungen → Autorisierte Domains → „Domain hinzufügen“** → `DEIN-BENUTZERNAME.github.io` eintragen. Damit funktionieren auch die E-Mails für „Passwort vergessen“ zuverlässig.

## Schritt 7: Freunde einladen

Schick den Link herum. Jede Person klickt auf „Registrieren“ (Name, E-Mail, Passwort) und hat danach ihr eigenes Dashboard. In der Gruppe taucht sie auf, sobald sie ein Ziel bewertet oder ihre Verfügbarkeit einträgt.

## Häufige Probleme

- **„Firebase ist noch nicht eingerichtet“:** `config.js` ist noch nicht ausgefüllt oder wurde nicht mit hochgeladen.
- **„Keine Berechtigung“:** Die Regeln aus Schritt 3 fehlen, oder die E-Mail steht bei Variante B nicht in der Liste.
- **„E-Mail-Anmeldung ist nicht aktiviert“:** Schritt 2 nachholen.
- **Leere Seite beim Doppelklick auf `index.html`:** Browser blockieren das lokal. Entweder über GitHub Pages öffnen oder im Ordner `python3 -m http.server` starten und `http://localhost:8000` aufrufen.
- **Änderungen sind nicht sichtbar:** GitHub Pages braucht nach dem Hochladen 1–2 Minuten; danach die Seite neu laden.

## So plant die Seite

Ziele mit der höchsten Gruppenbewertung werden zuerst eingeplant und geografisch sinnvoll in die Route einsortiert, bis der Zeitraum voll ist. Fixstopps werden immer eingeplant, mit Datum an genau diesem Tag. Ab 500 km Luftlinie zwischen zwei Stopps rechnet die Seite einen Reisetag. Mitreisend ist bei einem Stopp, wer das Ziel mit „Okay“ oder besser bewertet hat und im ganzen Zeitraum des Stopps verfügbar ist. Das ist eine Heuristik, kein mathematisch garantiertes Optimum.

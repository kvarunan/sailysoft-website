# SailySoft.de auf DigitalOcean App Platform deployen

Kosten: **0 €** (App Platform Starter-Tier, bis 3 statische Seiten kostenlos). SSL-Zertifikat inklusive, automatisch.

## Schritt 1: GitHub-Repository anlegen

1. Auf https://github.com/new gehen
2. Repository-Name: `sailysoft-website`
3. Sichtbarkeit: **Private** (reicht aus, App Platform kann auch private Repos lesen)
4. Ohne README erstellen → **Create repository**

## Schritt 2: Dateien hochladen

Im Terminal, im Ordner `deploy`:

```bash
cd "/Users/varun/Claude/Projects/SailySoft Webseite/deploy"
git init
git add .
git commit -m "SailySoft Webseite"
git branch -M main
git remote add origin https://github.com/kvarunan/sailysoft-website.git
git push -u origin main
```

`kvarunan` durch deinen GitHub-Benutzernamen ersetzen (auch in `.do/app.yaml`).

## Schritt 3: App Platform einrichten

1. https://cloud.digitalocean.com → **Create** → **App Platform**
2. **GitHub** als Quelle wählen → DigitalOcean autorisieren → Repo `sailysoft-website` auswählen
3. Branch: `main`, Autodeploy: **aktiviert lassen**
4. DigitalOcean erkennt die Seite automatisch als **Static Site** — falls nicht, Ressourcentyp manuell auf "Static Site" stellen
5. Region: **Frankfurt (fra)**
6. Plan: **Starter (Free)**
7. **Create Resources** klicken

Nach 1–2 Minuten ist die Seite unter einer URL wie `sailysoft-website-xxxxx.ondigitalocean.app` live. Erst testen, dann Domain umstellen.

## Schritt 4: Domain sailysoft.de verbinden

1. In der App: **Settings** → **Domains** → **Add Domain**
2. `www.sailysoft.de` eintragen → als **Primary** setzen
3. `sailysoft.de` zusätzlich hinzufügen (wird automatisch auf www umgeleitet — ersetzt die alte .htaccess-Regel)
4. DigitalOcean zeigt dir die nötigen DNS-Einträge an. Beim Domain-Anbieter (wo sailysoft.de registriert ist) eintragen:
   - `www` → **CNAME** auf die angezeigte `*.ondigitalocean.app`-Adresse
   - `@` (Root) → **A-Record** auf die angezeigte IP bzw. ALIAS/ANAME falls unterstützt
5. Warten (DNS-Umstellung: Minuten bis wenige Stunden). SSL-Zertifikat erstellt DigitalOcean automatisch, sobald DNS stimmt.

## Spätere Änderungen

Datei ändern → committen → pushen:

```bash
cd "/Users/varun/Claude/Projects/SailySoft Webseite/deploy"
git add .
git commit -m "Text angepasst"
git push
```

App Platform deployt automatisch neu (ca. 1 Minute).

## Hinweise

- Die alte `.htaccess` wird nicht mehr gebraucht: HTTPS erzwingt App Platform immer, die www-Weiterleitung übernimmt die Domain-Einstellung, Komprimierung und CDN-Caching sind eingebaut.
- `.do/app.yaml` ist optional (die Einrichtung über die Weboberfläche reicht), dokumentiert aber die Konfiguration.

# 🎁 Das Escape-Spiel an eine andere Lehrperson weitergeben

> Dieses Klassen-Escape funktioniert wie das WWM-Spiel: Es liegt als Webseite auf
> **GitHub Pages** und verbindet Host & Spielgeraete ueber einen **eigenen kleinen
> Broker** auf **Render.com** (statt ueber den oeffentlichen `0.peerjs.com`, der
> eine ganze Klasse hinter einer Schul-IP mit `HTTP 429` drosselt). Zusaetzlich
> sorgt **Metered.ca** (TURN) fuer die Durchleitung, wenn noetig.

## Drei Gratis-Dienste

| Dienst | Wofuer | Konto noetig? |
|--------|--------|---------------|
| **GitHub Pages** | hostet die Webseite (host/player) | ja (gratis) |
| **PeerJS-Broker** (auf Render) | verbindet Host & Spieler | ja (gratis) |
| **Metered.ca** (TURN) | leitet die Verbindung weiter, wenn noetig | ja (gratis) |

Auf deinem eigenen Computer muss **nichts** laufen.

---

## Zwei Wege

| Situation | Welcher Weg? |
|-----------|--------------|
| Kollegin im selben Haus, gelegentlich | **Weg A** – meinen Link & meine Dienste mitnutzen. Kein Einrichten. |
| Person soll dauerhaft & eigenstaendig sein | **Weg B** – eigene Kopie, eigener Broker, eigenes Metered-Konto. |

---

# Weg A: Einfach mitnutzen (kein Einrichten)

Du gibst der Person nur deinen Host-Link, z. B.
`https://romaneigenmann88-jpg.github.io/escape-room-classroom/host.html`.
Sie spielt los und nutzt deinen Broker und dein Metered-Konto mit.

**Einzige Pflicht:** Der Render-Gratis-Broker schlaeft nach 15 Min Ruhe ein. Wecke
ihn **1–2 Minuten vor der Stunde** auf, indem du diese Adresse einmal oeffnest:

> **https://wwm-peer-broker.onrender.com**

Kommt dort eine kurze Zeile `{"name":"PeerJS Server", ...}`, ist er wach. ✅

➡️ Fragen/Raeume weitergeben: siehe ganz unten.

---

# Weg B: Voellig eigenstaendig

## Schritt 1: Eigene Kopie der App auf GitHub
1. GitHub-Konto erstellen: https://github.com/signup
2. Meine Projektseite: `https://github.com/romaneigenmann88-jpg/escape-room-classroom`
3. Oben rechts **„Fork"** → eigene Kopie.
4. Im Fork: **Settings → Pages** → Source: Branch **`master`**, Ordner **`/ (root)`** → **Save**.
5. Nach 1–2 Min online: `https://DEIN-BENUTZERNAME.github.io/escape-room-classroom/host.html`

## Schritt 2: Eigener PeerJS-Broker auf Render
1. Broker-Vorlage forken: `https://github.com/romaneigenmann88-jpg/wwm-peer-broker` → **Fork**.
2. Auf https://render.com mit GitHub anmelden → **New +** → **Web Service** →
   Repo `wwm-peer-broker` waehlen → **Build** `npm install`, **Start** `npm start`,
   **Plan: Free** → **Create**.
3. Warten bis **„Live"**, URL kopieren (z. B. `https://wwm-peer-broker-xxxx.onrender.com`).
4. In **`host.html`** UND **`player.html`** nach **`PEER_CONFIG`** suchen (Strg+F):

```js
const PEER_CONFIG = { host:'wwm-peer-broker.onrender.com', port:443, path:'/', secure:true };
```

   Nur den **`host`**-Wert auf die eigene Render-URL setzen – **ohne** `https://`
   und **ohne** Schraegstrich am Ende. **In beiden Dateien gleich.**

⚠️ Auch hier: Broker 1–2 Min vor der Stunde einmal oeffnen (Kaltstart ~30–50 s).

## Schritt 3: Eigenes Metered.ca-Konto (TURN)
1. Kostenlos registrieren: https://www.metered.ca/ → **Sign up** → TURN-Service anlegen.
2. Im Dashboard die **statischen TURN-Zugangsdaten** holen: `username` + `credential`.
3. In **`host.html`** UND **`player.html`** die TURN-Zugangsdaten ersetzen. Sie stehen
   hier direkt im `new Peer(...)`-Aufruf (im `iceServers`-Block) und kommen **mehrfach**
   vor. Am einfachsten per Suchen-und-Ersetzen **alle Vorkommen** dieser beiden Werte
   austauschen:

   | Suchen (alter Wert) | Ersetzen durch |
   |---------------------|----------------|
   | `c4ca643ec3ef89475fda8aaf` | dein `username` aus Metered |
   | `958T4scRQJRKgSyg` | dein `credential` aus Metered |

   Tipp: Den Code-Editor auf dem PC (z. B. VS Code) nutzen und „Alle ersetzen" –
   dann sicher kein Vorkommen vergessen. Danach Dateien committen/hochladen.

## ✅ Werte-Checkliste fuer Weg B

| Wert | Woher | Wohin (host.html **und** player.html) |
|------|-------|----------------------------------------|
| Broker-URL | Render (Schritt 2) | `PEER_CONFIG.host` |
| TURN-Benutzer | Metered-Dashboard | alle Vorkommen von `c4ca643ec3ef89475fda8aaf` |
| TURN-Passwort | Metered-Dashboard | alle Vorkommen von `958T4scRQJRKgSyg` |

Einmalig einstellen: GitHub **Pages** auf `master` / `/ (root)`, Render-Plan **Free**.

---

## Raeume / Fragen weitergeben

Das Escape laedt seine Raeume aus einer **JSON-Datei** (z. B. `schwangerschaft-escape.json`).
So gibst du deinen Inhalt weiter:
1. Die JSON-Datei (oder den JSON-Text aus dem Editor) der Person schicken.
2. Sie oeffnet bei sich **`host.html`** und importiert/fuegt die JSON ein.
3. Die Raeume erscheinen sofort in der Vorschau.

---

## Haeufige Fragen

**Muss bei mir ein Server laufen?**
→ Nein. Broker (Render) und TURN (Metered) liegen in der Cloud. Sobald die App auf
GitHub Pages liegt, laeuft alles ohne deinen Computer.

**Der erste Beitritt dauert lange / klappt nicht.**
→ Der Render-Gratis-Broker war eingeschlafen. Broker-URL 1–2 Min vorher oeffnen.

**Welcher Browser?**
→ Chrome oder Edge.

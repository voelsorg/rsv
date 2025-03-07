# Radstammtisch Völs Source der Webseite

## Grundsaetzliche Vorbereitungen

**TL;DR** Du musst [uv](https://docs.astral.sh/uv/) installiert haben und das Git-Repository unter https://github.com/voelsorg/rsv geklont haben. Dann gehe in die Kommandozeile in den geklonten Ordner.

### Git - Eine Einführung für Anfänger

#### Voraussetzungen

Bevor du loslegen kannst, benötigst du Folgendes:

1. Ein Benutzerkonto bei [GitHub](https://github.com/signup).
2. Schreibzugriff auf das gewünschte Repository auf GitHub. Falls du diesen noch nicht hast, kontaktiere Jens per E-Mail oder in unserer Signal-Gruppe.
3. Eine Git-Installation auf deinem Computer. Es gibt zwei Möglichkeiten:
   - **Kommandozeilen-Tool**: [Git installieren](https://docs.github.com/de/get-started/getting-started-with-git/set-up-git).
   - **Grafische Oberfläche**: Falls du eine einfachere Bedienung bevorzugst, kannst du ein Tool mit grafischer Benutzeroberfläche verwenden, z. B.:
     - [GitHub Desktop](https://github.com/apps/desktop) (empfohlen für Mac und Windows, speziell für GitHub)
     - [Sourcetree](https://www.sourcetreeapp.com/)
     - Weitere Optionen findest du [hier](https://git-scm.com/downloads/guis).

#### Was ist Git?

Git ist ein Versionskontrollsystem. Es erlaubt mehreren Personen, gleichzeitig an einem Repository (Projekt) zu arbeiten, ohne sich gegenseitig zu behindern. Die zentrale Kopie des Projekts liegt auf GitHub.

##### Grundlegender Arbeitsablauf

1. **Repository klonen**: Beim ersten Mal musst du eine Kopie des Repositories auf deinen Computer herunterladen. Dies nennt man **klonen** (clone).

2. **Änderungen vornehmen**: Du bearbeitest die Dateien in deinem lokalen Repository auf deinem Computer.

3. **Änderungen speichern (commit)**: Sobald du mit deinen Änderungen zufrieden bist, speicherst du sie in Git. Dies geschieht mit einem **Commit**, der eine neue Version der Datei erstellt – allerdings nur lokal auf deinem Computer!

4. **Änderungen hochladen (push)**: Damit deine Änderungen auch für andere sichtbar werden, musst du sie in das zentrale Repository auf GitHub hochladen.

5. **Änderungen anderer herunterladen (pull)**: Falls andere in der Zwischenzeit Änderungen an den gleichen Dateien vorgenommen haben, kannst du diese mit **pull** in dein lokales Repository herunterladen.

##### Konflikte (Merge Conflicts)

Falls du und jemand anderes gleichzeitig an der gleichen Datei (insbesondere an den gleichen Zeilen) gearbeitet habt, kann es zu einem **Merge-Konflikt** kommen. Dieser muss zuerst manuell gelöst werden, bevor du weiterarbeiten kannst.


## Arbeiten mit dem Repository

### Repository klonen

#### Über die kommandozeile

Es gibt zwei Möglichkeiten, ein Repository über die Kommandozeile zu klonen. Wechsle dazu zuerst in das gewünschte Verzeichnis, in dem du das Repository speichern möchtest:

```bash
cd ~/Documents
```

1. **Per HTTP (empfohlen):**
   ```bash
   git clone https://github.com/voelsorg/rsv.git
   ```
2. **Per SSH (nur falls du SSH bereits nutzt oder lernen möchtest):**
   ```bash
   git clone git@github.com:voelsorg/rsv.git
   ```

Nach dem Klonen musst du in das neu erstellte Verzeichnis wechseln:

```bash
cd rsv
```

#### Mit grafischer Oberfläche Github Desktop

Falls du eine grafische Oberfläche verwendest, gibt es in den meisten Programmen eine Funktion wie "Repository klonen" oder "Clone Repository".
Öffne diese Funktion und füge die HTTP- oder SSH-Adresse des Repositories in das entsprechende Feld ein.
In GitHub Desktop beispielsweise klickst du auf "Clone a repository", wählst "URL" aus, fügst die Repository-URL ein und bestimmst den Speicherort auf deinem Computer.
In anderen Programmen wie Sourcetree oder GitKraken funktioniert dies ähnlich.

### Notwendige Werkzeuge installieren

Für die Arbeit mit dem Site-Generator benötigst du das Tool [uv](https://docs.astral.sh/uv/). Installiere es unter Linux mit:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Erstellen oder Bearbeiten von Inhalten

Du befindest dich nun in der Kommandozeile im `rsv`-Ordner.

Die Webseite wurde mit der Software Lektor erstellt.
Lektor ist ein statischer Webseiten-Generator, der es ermöglicht, Webseiten einfach zu verwalten und zu bearbeiten, ohne direkt in den Code eingreifen zu müssen.
Inhalte werden über eine benutzerfreundliche Web-Oberfläche verwaltet, während Lektor im Hintergrund die statischen HTML-Dateien generiert.
Um die Bearbeitung zu starten, verwende folgenden Befehl:

```bash
uvx lektor serve
```

Dann öffne im Browser die folgende Adresse:

[http://127.0.0.1:5000/](http://127.0.0.1:5000/)

Zunächst siehst du die Webseite deiner lokalen Arbeitskopie. Klicke oben rechts auf das Stift-Symbol, um in den Bearbeitungsmodus zu wechseln.



### Änderungen an GitHub zurücksenden

#### Kommandozeile

Sobald du deine Änderungen an der Webseite vorgenommen hast und zufrieden bist, musst du diese wieder in das zentrale Repository auf GitHub hochladen. Dies geschieht in drei Schritten:

1. **Änderungen zum Commit hinzufügen**: Zunächst fügst du alle geänderten Dateien zur Git-Überwachung hinzu:

   ```bash
   git add .
   ```

   Der Punkt (`.`) bedeutet, dass alle geänderten Dateien in diesem Verzeichnis hinzugefügt werden. Alternativ kannst du auch einzelne Dateien angeben, z. B. `git add dateiname.txt`.

2. **Änderungen committen**: Nun erstellst du einen Commit, der eine neue Version deiner Änderungen speichert:

   ```bash
   git commit -m "Meine Änderungen an der Webseite"
   ```

   Die Nachricht im Anführungszeichen sollte eine kurze Beschreibung der vorgenommenen Änderungen enthalten.

3. **Änderungen nach GitHub hochladen**: Abschließend sendest du die Änderungen an das zentrale Repository:

   ```bash
   git push
   ```

   Falls du nach einem Benutzernamen und Passwort gefragt wirst, gib deine GitHub-Zugangsdaten ein oder nutze ein persönliches Zugriffstoken, falls du eine Zwei-Faktor-Authentifizierung verwendest.

Auf Github wird die Seite dann automatisch in HTML gebaut und veröffentlicht.


#### Änderungen mit grafischen Werkzeugen hochladen

Falls du Änderungen mit einer grafischen Oberfläche verwaltest, funktioniert das Hochladen ähnlich:

1. Dateien überwachen: In GitHub Desktop oder Sourcetree siehst du eine Liste der geänderten Dateien. Wähle die Dateien aus, die du committen möchtest.

2. Commit erstellen: Gib eine kurze Beschreibung der Änderungen ein und klicke auf "Commit".

3. Änderungen hochladen: Klicke auf "Push", um die Änderungen in das zentrale Repository auf GitHub hochzuladen.

Auf Github wird die Seite dann automatisch in HTML gebaut und veröffentlicht.

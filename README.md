# Radstammtisch Völs Source der Webseite

## Grundsaetzliche Vorbereitungen

**TL;DR** Du musst [uv](https://docs.astral.sh/uv/) installiert haben und das Git-Repository unter https://github.com/voelsorg/rsv geklont haben. Dann gehe in die Kommandozeile in den geklonten Ordner.

### Git

1. Du brauchst ein Benutzer-Konto bei [GitHub].
2. Du brauchst Schreibzugreif auf das Repository bei Github. Kontaktiere dazu @jensens.
3. Du musst bei Dir am Computer [git installieren](https://docs.github.com/de/get-started/getting-started-with-git/set-up-git)
   Entweder direkt das Kommandozeilen Git oder ein Programm, welches eine grafische Oberflaeche dafuer zur Verfuegung stellt und die Bedienung damit vereinfacht.
   Ich empfehle Anfaengern solch ein Programm zu verwenden.
   Fuer Mac und Windows gibt speziell fuer Github den [Github Desktop](https://github.com/apps/desktop).
   Allgemein fuer Git gibt es sonst noch [Sourcetree](https://www.sourcetreeapp.com/) oder [andere](https://git-scm.com/downloads/guis).

Git ist ein Versionskontrollsystem.
Das heisst man kann mit mehrere Personen an einem Repository arbeiten ohne sich gross in die Quere zu kommen.
Die Hauptkopie der Webseite, das Repository, liegt auf Github.

- Mit Git holt man sich erstmalig eine lokale Arbeitskopie des Repositories auf seinen Computer herunterladen, das nennt man klonen (clone).
- Am eigenen Computer bearbeitet man dann diese Kopie.
- Wenn fertig, dann fuegt man seine Aenderungen der lokalen Arbeitskopie dem Git hinzu (commit).
  Die erzeugt automatisch eine neue Version - aber erstmal nur auf dem eigenen Computer!
- Anschliessend muss man die neue Version zu Github hochladen (push).

Wenn jemand anders am Github Aenderungen (neue Versionen) gemacht hat und man die in seiner lokale Arbeitskopie haben will, muss man diese herunterladen.
Da man aber schon einen Klon hat, muss man hier nur die Aenderungen ziehen (pull).

Wenn man Pech hat, hat man selber und jemand anders an der selben Datei gebarbeitet und bei Textdateie an den selben Zeilen.
Dann kann es einen Konflikt geben (merge conflict). Dieser muss erst aufgeloest werden, dann kann man weiterarbeiten.

### Arbeiten mit dem Repository

Klone das Repository.

Hier gibt es zwei Varianten das ueber die Kommandozeile zu loesen.

Gehe dazu erst in den Ordner, in den Du das Repository klonen willst. z.B.
```bash
cd Documents
```

1. per HTTP (empfohlen)

   ```bash
   git clone https://github.com/voelsorg/rsv.git
   ```

2. per SSH (nur verwenden, wenn Du dich schon mit SSH auskennst oder es lernen willst)
   ```
   git clone https://github.com/voelsorg/rsv

Dann gehe in den neu geklonten Order mit dem Projektdateien:

```bash
cd rsv
```

Wenn Du eine grafische Oberflaeche verwendest suche nach einer Funktion um eine Repository zu klonen und verwende den letzten der drei Teile des obigen Kommandos.

In jedem Fall brauchst Du noch ein Werkzeug um mit dem Site-Generator zu arbeiten, installiere [uv](https://docs.astral.sh/uv/) - unter Linux mit

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Erstellen oder Bearbeiten von Inhalten

Du bist in der Kommandozeile im `rsv` Ordner.

Die Webseite wurde mit der Software [Lektor](https://www.getlektor.com/) gebaut.

Starte die Bearbeitung mit

```bash
uvx lektor serve
```

Und gehe im Browser auf http://127.0.0.1:5000/

Erst siehst Du Die Webseite der lokalen Arbeitskopie.

Oben rechts ist ein Stift-Icon, dort wechselt Du in den Bearbeiten-Modus.


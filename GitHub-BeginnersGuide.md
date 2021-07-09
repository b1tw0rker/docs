**Ein existierendes Projekt nach Github migrieren**

Ein neues Repository auf github.com erstellen. Nicht README, license
oder .gitignore initialisieren (Siehe Bild)

![](.//media/image1.jpeg){width="6.3in" height="4.9222222222222225in"}

**Achtung:** github liefert nach Erstellung eine Anleitung mit ‚main'
als branch. Wir wollen aber ‚master' als branch. Entsprechend ist das
Script angepasst.

cd /lokales/projektverzeichnis

Em Ende fragt github.com nach Usernamen und Password, falls es nicht im
askpass verfügbar ist.

![](.//media/image2.jpg){width="6.3in" height="2.3743055555555554in"}

**Git.sh**\
Mit diesem kleinen Script kannst du Änderungen automatisch nach
github.com auschecken.

Füge in .gitignore ‚git.sh' zu, damit die Datei nicht mit ins Repository
ausgecheckt wird.

Wenn das Passwort nicht erneut eingegeben werden soll, kann das mit
folgendem Befehl gelöst werden:

Git wird die benötigten Passwörter aus dem loklane, globalen Cache
holen.

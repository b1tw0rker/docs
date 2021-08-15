# Ein existierendes Projekt nach Github migrieren

Ein neues Repository auf github.com erstellen. Nicht README, license
oder .gitignore initialisieren (Siehe Bild)

![Create an new Repo](.//media/image1.jpeg)


**Achtung:** github liefert nach Erstellung eine Anleitung mit ‚main'
als branch. Wir wollen aber ‚master' als branch. Entsprechend ist das
Script angepasst.

```bash
cd /lokales/projektverzeichnis
```

```bash
echo "# vsftpd" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/USERNAME/name-des-repository.git
git push -u origin master
```



Em Ende fragt github.com nach Usernamen und Password, falls es nicht im
askpass verfügbar ist.

```bash
(gnome-ssh-askpass:30112): Gtk-WARNING **: 17:12:10.921: cannot open display:
error: unable to read askpass response from '/usr/libexec/openssh/gnome-ssh-askpass'
Username for 'https://github.com': bitw0rker

(gnome-ssh-askpass:30113): Gtk-WARNING **: 17:12:22.007: cannot open display:
error: unable to read askpass response from '/usr/libexec/openssh/gnome-ssh-askpass'
Password for 'https://bitw0rker@github.com':
```


### git.sh
Mit diesem kleinen Helper Script kannst du Änderungen automatisch nach github.com auschecken.

cd /your/lokal/projectfolder

```bash
#!/bin/bash
git add *
git commit -m "initial version"
git push origin master
exit 0
```

Füge in .gitignore ‚git.sh' zu, damit die Datei nicht mit ins Repository
ausgecheckt wird.


Wenn das Passwort nicht erneut eingegeben werden soll, kann das mit
folgendem Befehl gelöst werden:


```bash
git config credential.helper store
# Noisy Warnmeldung loswerden
git config advice.addIgnoredFile false
```


Git wird die benötigten Passwörter aus dem loklane, globalen Cache
holen.

### Token 

Seit dem 14.8.2021 akzeptiert github keine Passwörter mehr zur Übertragung der Daten. Du musst ein Token im Bereich:

```bash
Settings, Developer Seetings, Personal access token
```
erzeugen.

Speicher das Token lokal.
Bereits bestehende lokalte git Repositories werden wie folgt upgedatet. (Läuft dann auch wieder mit Visual Studio Code)

```bash
git remote set-url origin https://username:token@github.com/username/repository.git
```
Deine lokale Datei .git/config ändert sich dadurch wie folgt ab:

```bash
[remote "origin"]
	url = https://username:TOKEN@github.com/username/repository.git
```

Du kannst also auch die lokale .git\config Datei manipulieren.

Quelle: https://stackoverflow.com/questions/66231282/how-to-add-github-personal-access-token-to-visual-studio-code

Einen manuellen Push erzeigt du wie folgt:

```bash
https://stackoverflow.com/questions/18935539/authenticate-with-github-using-a-token
```
Quelle: https://stackoverflow.com/questions/18935539/authenticate-with-github-using-a-token

Auf keinen Fall soltest du dein Token im Ordner .git oder im Repository speichern !!!

### License
[MIT](https://choosealicense.com/licenses/mit/)

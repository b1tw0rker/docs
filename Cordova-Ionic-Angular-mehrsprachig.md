# Cordova/Ionic/Angular App mehrsprachig mit der Internationalization API.

<a href="https://www.bit-worker.com/web/blog-cordova-ionic-angular-internationalization-api.html" target="_blank">Globalization Plugin Artikel online auf BitWorker</a>


Requirements:
Cordova | Ionic | Angular | JQuery
             
Auf der Suche im Web, nach einer vernünftigen Localization-Lösung für Cordova, Ionic bzw. Angular war ich nicht erfolgreich.
Oops, so viel neue Technik und so schlechte Doku im Netz. Na gut..., muss ich es mir halt selbst besorgen...
Also fing ich an zu coden und das kam dabei raus:

Es gibt natürlich jede Menge varalteter Artikel (z.B. aus dem Jahr 2014) im Web. Diese beschreiben aber ebend auch eine veraltete Technik...
Ich versuche mit diesem Artikel eine moderne Variante für Übersetzungen aufzuzeigen, die aber trotzdem "lightway" ist.

### Vorab:
Zur Localization von Cordova-Apps wird das <a href="https://github.com/apache/cordova-plugin-globalization" target="_blank">Globalization Plugin</a> von Apache/Cordova nicht mehr benötigt.
Das Plugin wird über kurz oder lang aus dem Plugin-Repository von Cordova und Ionic rausfliegen.
Moderne Browser liefern eine Localization von Haus aus mit. Und mit meiner Lösung existiert immer noch ein Fallback,
falls der Browser wider Erwarten doch keine Localization ausliefert.
Das Plugin bzw. die Technik, die der Browser mitliefert, nennt sich: „<a href="https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Intl" target="_blank">Internationalization API</a>“.

Die Programmierung verlief früher wie folgt: In Cordova wurde das Globalization-Plugin implementiert und mit folgendem Script ermittelt welche
Sprache der lokale Browser verwendet:

```bash
navigator.globalization.getPreferredLanguage(function (language) {
console.log('language: ' + language.value + '\n');
}, function () {
console.log('Error getting language\n');
});
```

        
 Auf dieses Plugin kann nun komplett verzichtet werden.
 Siehe dazu auch hier: <a href="https://cordova.apache.org/news/2017/11/20/migrate-from-cordova-globalization-plugin.html" target="_blank">Migration from the Globalization Plugin</a>
 Die Internationalization API von Browser lässt sich stabil ausführen, wenn (wie üblich) das „deviceready“ Statement "abgefeuert" wurde.
 

### Check this out:

```bash
document.addEventListener("deviceready", function() {
if (window.Intl && typeof window.Intl === 'object') {

console.log("%cAPI available. Detected Lan: " + navigator.language + "", "color: green");
alert(navigator.language);

}
}, false);
```

Je nach Sprache und Browser liefert navigator.language verschiedene Landeskürzel (de, de-DE, en, en-US, fr, fr-FR etc.).
Daraus extrahiere ich eine Variable, mit der später die entsprechende JSON-Datei, mit den eigentlichen Übersetzungen, geladen wird.


```bash
document.addEventListener("deviceready", function() {

if (window.Intl && typeof window.Intl === 'object') {

console.log("%cAPI available. Lan: " + navigator.language + "", "color: green");

// URL implementieren
if (navigator.language == "de" || navigator.language == "de-DE") {
var url = "i18n/de.json";
} else if (navigator.language == "en" || navigator.language == "en-US") {
var url = "i18n/en.json";
} else {
// Fallback für alle anderen Sprachen
var url = "i18n/en.json";
}

} else {
// Fallback - Falls die API keine Sprache liefert.
var url = "i18n/en.json";
}


seti18n(url); // fire Language-Chooser function with detected url

}, false);

```

     

Die Übersetzungen befinden sich im Ordner: www/i18n/ und heißen de.json, en.json, etc.
<img src="assets/img/blog/i18n.jpg" style="width: 350px;" alt="json - ordner der einzelnen dateien">

Die jeweilige JSON-Datei hat folgendes Format:
JSON-Datei:

```bash
{
"Strings": [
{
"username": "Gib deinen Usernamen ein.",
"password": "Gib dein Passwort ein.",
"headingtext": "Überschrift Deutsch",
"submit": "Abschicken"
}
]
}
```

         

          
Vorbereitung der HTML-Datei:

Alle Tags, die potentiell übersetzt werden sollen, bekommen die Klasse ‚i18n‘ sowiedas ‚data-i18n‘ Attribut verpasst.
Nun kommt der nächste Fallback ins Spiel. Kann die JSON-Datei aus irgend einem Grund nicht geladen werden, wird immer noch der Node-Wert aus dem HTML-Tag verwendet.

Beispiel eines Button-Tag:

&lt;button class=&quot;i18n&quot; data-i18n=&quot;submit&quot; type=&quot;submit&quot;&gt;Abschicken&lt;/button&gt;


Das data-i18n Attribut definiert, welches JSON-Objekt eingefügt werden soll. In dem Fall unseres Beispiel Buttons ist dies das Objekt „submit“ --> Also: "submit": "Abschicken"

Let's Check the "seti18n" Function:

```bash
function seti18n(url) {

$.getJSON(url, function(data){
    $(".i18n").each(function(){
    $(this).html(data.Strings[0][$(this).data("i18n")]);
    $(this).attr("placeholder",data.Strings[0][$(this).data("i18n-ph")]); // i18n-ph für placeholder Übersetzungen only
    $(this).attr("value",data.Strings[0][$(this).data("i18n-val")]); // i18n-val für input value Übersetzungen only
  });
});

}

```

Gar nicht so spektugal eigentlich :-)
Die Funktion "seti18n" lädt per $.getJSON Befehl, die entsprechende JSON-Datei und <i>"parkt"</i> den Inhalt in der Variable ‚data‘.
Anschließend werden sämtliche i18n Klassen mit dem JQuery Befehl: $(".i18n").each(function() identifiziert, und die JSON-Strings entsprechend eingesetzt.
 
Eine kleine, knackige Lösung, wie ich finde.
 
```bash
    AUSBLICK: Mit dieser Lösung könnte man mit einem einfachen $.ajax Call selbstverständlich die JSON-Datei auch auf einem externen Server auslagern und somit (völlig unabhängig von der App)
    Übersetzungen und Aktualisierungen bearbeiten…
```

### License
[MIT](https://choosealicense.com/licenses/mit/)

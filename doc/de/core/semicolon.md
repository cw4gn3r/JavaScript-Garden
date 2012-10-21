## Automatisches Semikolon

Auch wenn die Syntax von JavaScript sehr an Sprachen wie C oder Java erinnert, ist die Benutzung des Semikolon tatsächlich *nicht* erforderlich. Man kann diese also auch einfach weglassen.

Das heißt allerdings nicht, dass das Semikolon keine Rolle spielt. Hinter den Kulissen braucht der Parser es tatsächlich, um den Quellcode zu verstehen. Der Parser setzt daher **automatisch** ein Semikolon ein, falls dessen Nichtvorhandensein einen Syntaxfehler versursachen würde.

    var foo = function() {
    } // Syntaxfehler, Semikolon fehlt
    test()

An dieser Stelle setzt der Parser ganz einfach ein Semikolon ein, und versucht es 
noch einmal:

    var foo = function() {
    }; // kein Fehler
    test()

Dieses automatische Einsetzen von Semikolons wird häufig als der größte 
Designfehler von JavaScript angesehen, da das Risiko besteht, den erwarteten
Programmablauf zu beeinflussen. 

### Wie es funktioniert

Der folgende Quellcode hat keinerlei Semikolons, daher bestimmt der Parser von allein, wo diese hinzufügt werden.

    (function(window, undefined) {
        function test(options) {
            log('testing!')

            (options.list || []).forEach(function(i) {

            })

            options.value.test(
                'ein langer String',
                'und noch ein langer String'
            )

            return
            {
                foo: function() {}
            }
        }
        window.test = test

    })(window)

    (function(window) {
        window.someLibrary = {}

    })(window)

Hier ist das Resultat dieses "Ratespiels":

    (function(window, undefined) {
        function test(options) {

            // Kein Semikolon hinzugefügt, Zeilen wurden zusammengeführt
            log('testing!')(options.list || []).forEach(function(i) {

            }); // <- hinzugefügt

            options.value.test(
                'ein langer String',
                'und noch ein langer String'
            ); // <- hinzugefügt

            return; // <- hinzugefügt
            { // wird als Block interpretiert

                // ein Label und eine Ausdrucksanweisung
                foo: function() {} 
            }; // <- hinzugefügt
        }
        window.test = test; // <- hinzugefügt

    // Diese Zeilen wurden auch zusammengeführt
    })(window)(function(window) {
        window.someLibrary = {}; // <- hinzugefügt

    })(window); //<- hinzugefügt

> **Hinweis:** `return` Answeisungen die von einem Zeilenumbruch gefolgt werden, 
> werden vom Parser nicht "korrekt" behandelt. Dies muss nicht zwinged als Fehler
> angesehen werden, kann allerdings dennoch zu ungewollten Nebeneffekten führen.

Im obigen Fall hat der Parser unseren Quellcode drastisch verändert. In manchen
Fällen führt dies zu ungewollten Konsequenzen.

### Führende Klammer

Im falle einer führenden Klammer im nächsten Ausdruck wird vom Parser **kein** Semikolon eingesetzt:

    log('testing!')
    (options.list || []).forEach(function(i) {})

Diese zwei Zeilen werden zu einer einzigen Zeile vereinfacht:

    log('testing!')(options.list || []).forEach(function(i) {})

**Höchstwahrscheinlich** liefert unsere `log` Funktion **nicht** eine Funktion 
als Rückgabewert. In diesem Fall wird das Ausführen des obigen Beispiel zu einem
`TypeError` führen, mit dem Hinweise `undefined is not a function`.

### Zusammenfassung

Wir empfehlen **dringend**, niemals ein Semikolon auszulassen, um bösen Überraschungen vorzubeugen. Weiterhin empfehlen wir, geschweifte Klammern immer
in der selben Zeile wie die dazugehörige Anweisung zu lassen, und auch nicht
in einziligen `if` / `else` Anweisungen darauf zu verzichten. Dies führt nicht nur zu einer besseren Lesbarkeit, sondern verhindert auch, dass der Parser den Quellcode in unerwarteter Weise verändert.


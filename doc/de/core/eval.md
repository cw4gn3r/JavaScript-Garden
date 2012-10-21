## Warum `eval` böse ist 

Die Funktion `eval` interpretiert einen String als ein Stück JavaScript und führt ihn im lokalen Geltungsbereich aus.

    var foo = 1;
    function test() {
        var foo = 2;
        eval('foo = 3');
        return foo;
    }
    test(); // 3
    foo; // 1

Allerdings hat die Sache einen Haken: der lokale Geltungsbereich kommt *nur dann* zur Anwendung wenn `eval` direkt aufgerufen wird (und nicht etwa mittels Alias):

    var foo = 1;
    function test() {
        var foo = 2;
        var bar = eval;
        bar('foo = 3');
        return foo;
    }
    test(); // 2
    foo; // 3

Aus diesem Grund raten wir davon ab, `eval` zu benutzen. 99.9% der Fälle in denen `eval` nützlich ist kann man auch ohne.
    
### `eval` in Verkleidung

Die [timeout Funktionen](#other.timeouts) `setTimeout` und `setInterval` können beide mit einem String als erstes Argument aufgerufen werden. Dieser string wird **immer** im globalen Geltungsbereich ausgeführt, da `eval` hier nicht direkt aufgerufen wird.

### Sicherheitslücken

`eval` kann ausserdem zu Sicherheitslücken führen, da es jeden Code ausführt, mit dem es aufgerufen wird. Es sollte daher **keinesfalls** mit Strings aus unbekannter Quelle (etwa Benutzereingaben) aufgerufen werden.

### Zusammenfassung

Die Benutzung von `eval` sollte soweit wie möglich umgangen werden. Jeglicher Code der `eval` benutzt, sollte genaustens untersucht werden, mit Rücksicht auf Sicherheit und Performance. Falls irgendwie möglich, sollte ganz auf die Benutzung verzichtet werden.

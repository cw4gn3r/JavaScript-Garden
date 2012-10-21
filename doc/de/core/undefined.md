## `undefined` und `null`

JavaScript hat zwei verschiene Werte, um den Zustand "nichts ist vorhanden" darzustellen: `null` und `undefined`. Von diesen ist der letztgenannte der hilfreichere. 

### Der Wert `undefined`

`undefined` ist tatsächlich ein Datentyp, der nur einen möglichen Zustand hat, nämlich `undefined`.

Darüberhinaus definiert JavaScript auch eine globale Variable mit dem Wert `undefined`; diese Variable heißt auch `undefined`. Allerdings handelt es sich hierbei **weder** um eine Konstante, noch um ein Schlüsselwort. Daher kann man den *Wert* von `undefined` auch ganz einfach überschreiben.

> **ES5 Hinweis:** Im "strict mode" von ECMAScript 5 ist `undefined` **nicht mehr**
> *überschreibbar*, allerdings kann dessen Sichtbarkeit immer noch beeinflußt werden,
> etwa duch die Definition einer Funktion or Variable mit demselben Namen.

Es folgen ein paar Beispiele, in denen der Wert `undefined` zurückgeliefert wird:

 - Lesen der (unmodifizierten) globalen Variable `undefined`.
 - Lesen einer bereits deklarierten, jedoch *nicht* initialisierten Variable.
 - Implizierter Rückgabewert einer Funktion ohne `return` Anweisung.
 - `return` Anweisungen ohne ausdrücklichen Rückgabewert.
 - Lesen einer nicht existenten Objekteigenschaft.
 - Lesen eines Funktionsparameters, dem kein Wert zugewiesen wurde.
 - Lesen einer Variable, der ausdrücklich der Wert `undefined` zugewiesen wurde.
 - Jeder Ausdruck in der Form `void(expression)`

### Wenn der Wert von `undefined` 

Da die globale Variable `undefined` nur eine *Kopie* des eigentlichen Wertes von `undefined` enthält, führt die Zuweisung eines neues Wertes von `undefined` **nicht** zu einer Änderung des *Datentyps* `undefined`.

Um allerdings etwas anderes mit dem Wert von `undefined` vergleichen zu können, müssen wir zuerst den Wert von `undefined` kennen.

Eine häufig verwendete Strategie, um ein Stück Quellcode gegen das Überschreiben von `undefined` abzusichern, ist, den Code in eine [anonyme Funktion](#function.scopes) einzubetten, und diese mit weniger Parametern aufzurufen:

    var undefined = 123;
    (function(something, foo, undefined) {
        // im lokalen Geltungsbereich hat undefined
        // nun wieder den Wert `undefined`

    })('Hallo Welt', 42);

Ein zweiter Weg zum selben Ergebnis ist es, `undefined` einfach als Variable zu deklarieren, ohne sie zu initialisieren:

    var undefined = 123;
    (function(something, foo) {
        var undefined;
        ...

    })('Hallo Welt', 42);

Der einzige Unterschied zwischen diesen beiden Vorhergehensweisen zeigt sich, wenn wir den Quellcode "minifizieren" wollen: in diesem Fall wird die zweite Version um 4 Bytes länger ausfallen, falls in der anonymen Funktion keine weitere Variable deklariert wird.

### Die Verwendung von `null`

Während `undefined` in JavaScript hauptsächlich wie das traditionelle *null* benutzt wird, ist der tatsächliche Wert `null` (sowohl als Literal als auch als Typbezeichnung) mehr oder weniger nur ein weiterer Datentyp.

Es spielt eine kleine Rolle in JavaScript Internas (wie etwa um das Ende der Prototypenkette anzuzeigen, indem man `Foo.prototype = null` setzt). In den allermeisten Fällen jedoch kann man es durch `undefined` ersetzten.



## Der `delete` Operator

Kurzfassung: es ist **nicht möglich**, globale Variablen, Funktionen und 
einige andere Dinge zu löschen, die ein `DontDelete` Attribut besitzten.

### Globaler Code und Funktionscode

Eine Variable oder Funktion, die entweder im globalen Geltungsbereich, oder im
[Geltungsbereich einer Funktion](#function.scopes) definiert wurde, ist eine
Eigenschaft der Objekte `Global` oder `Activation`. Solche Eigenschaften haben
eine Reihe von Attributen, von denen eine `DontDelete` heißt. Variablen- und
Funktionsdeklarationen in globalem Code oder Funktionscode erzeugen immer
Eigenschaften mit `DontDelete`, und können daher nicht gelöscht werden.

    // globale Variable:
    var a = 1; // DontDelete wurde gesetzt
    delete a; // false
    a; // 1

    // normale Funktion:
    function f() {} // DontDelete wure gesetzt
    delete f; // false
    typeof f; // "function"

    // Umzuweisung hilft nicht:
    f = 1;
    delete f; // false
    f; // 1

### Explizite Eigenschaften

Explizit gesetzte Eigenschaften können ganz normal gelöscht werden:

    // explizite Eigenschaft
    var obj = {x: 1};
    obj.y = 2;
    delete obj.x; // true
    delete obj.y; // true
    obj.x; // undefined
    obj.y; // undefined

Im obigen Beispiel können `obj.x` und `obj.y` nicht gelöscht werden, da diese
kein `DontDelete` attribut besitzen. Daher funktionert auch das folgende Beispiel:

    // dies funktioniert (außer im Internet Explorer):
    var GLOBAL_OBJECT = this;
    GLOBAL_OBJECT.a = 1;
    a === GLOBAL_OBJECT.a; // true - just a global var
    delete GLOBAL_OBJECT.a; // true
    GLOBAL_OBJECT.a; // undefined

Wir benutzen hier einen Trick, um die Variable `a` zu löschen. Im globalen
Kontext ist [`this`](#function.this) nämlich eine Referenz auf das globale 
Object und wir können `a` ausdrücklich als dessen Eigenschaft deklarieren, 
was dazu führt, dass wir es auch ganz normal wieder löschen können.

Internet Explorer (zumindest in den Versionen 6-8) hat einige Bugs, daher
funktioniert das Beispiel dort nicht.

### Funktionsargumente und eingebaute Eigenschaften

Funktionsargumente, [`arguments` Objekte](#function.arguments) und eingebaute
Eigenschaften besitzten auch ein `DontDelete` Attribut:

    // Funktionsargumente und Eigenschaften:
    (function (x) {
    
      delete arguments; // false
      typeof arguments; // "object"
      
      delete x; // false
      x; // 1
      
      function f(){}
      delete f.length; // false
      typeof f.length; // "number"
      
    })(1);

### Hostobjekte
    
Im Falle von Hostobjekten kann die Verhaltensweise des `delete` Operators 
mitunter schwer vorhersehbar sein. Die JavaScript Spezifikation beinhaltet
keine Vorschrift für diesen Fall, daher können solche Objekte auf verschiedenste
Weise implementiert sein.

### Zusammenfassung

Der `delete` Operator verhält sich häufig auf unerwartete Weise und kann nur 
zuverlässig benutzt werden, um ausdrückliche Eigenschaften "normaler" Objekte
zu löschen.

# Programmeren Basis - Deel PA1 - Oefeningen
## Inzicht in de werking verkrijgen
Nu je goed weet wat het programma kan, is het tijd om de code erbij te pakken.

Hieronder staan een aantal inzichtsvragen, je zult de code grondig moeten bestuderen om ze te kunnen beantwoorden.

> **Tip**
>
> Het kan daarbij verhelderend zijn om sommige stukken van het programma met de debugger te doorlopen om te zien wat er precies gebeurt!

**Code lezen en begrijpen is een belangrijke *skill* als programmeur. En ook hier geldt : van andermans antwoorden leer je niet veel, je moet het echt zelf doen.**

### Oefening DPA1.menu
Deze oefening gaat over het overkoepelende menu waarin de gebruiker moet kiezen wat hij/zij wil doen.

Ga na hoe het programma het menu toont en de gebruikersinput verwerkt.

1.  Waarom eindigt het programma pas als de gebruiker keuze `5` kiest?

2.  Wat bepaalt dat keuze `4` in het menu de 'alle details van een persoon zien' functionaliteit in gang steekt?

### Oefening DPA1.details
Deze oefening gaat over de menu keuze "alle details van een persoon zien".

Bekijk aandachtig de code van dit gedeelte en de gebruikte variabelen.

1.  Het programma gaat pas verder als je een geldige persoon gekozen hebt, waar gebeurt dit?

2.  Hoe is de informatie van één persoon over de verschillende arrays verdeeld?

3.  Waarvoor dient de variabele `aantalPersonen` in verband met de arrays?

4.  Hoe komt de initiële data in het programma terecht (i.e. de info over Jan en Mieke)?

5.  Hoe wordt de `true`/`false` waarde voor `isVrouw` omgezet naar `vrouw`/`man` in de output?

    -   Zie https://wellsb.com/csharp/beginners/csharp-ternary-conditional-operator/

### Oefening DPA1.toevoegen
Deze oefening gaat over de menu keuze "een persoon toevoegen".

Bekijk aandachtig de code van dit gedeelte en de gebruikte variabelen.

1.  Hoe zorgt het programma ervoor dat lege tekst niet aanvaard wordt als voornaam?

2.  Waarom hoeft er bij het geslacht niet op een lege string gecontroleerd te worden?

3.  Hoe wordt de `m`/`v` input bij geslacht, omgezet naar een `false`/`true` waarde voor `isVrouw`?

4.  Hoe wordt de ingevoerde data onthouden zolang het programma nog geen antwoord heeft op de `Wil u deze gegevens bewaren (j/n)?` vraag?

5.  Waarom moet deze onthouden data niet expliciet verwijderd worden als de gebruiker deze vraag met `n` beantwoordt?

### Oefening DPA1.aanpassen
Deze oefening gaat over de menu keuze "een persoon aanpassen".

Bekijk aandachtig de code van dit gedeelte en de gebruikte variabelen.

1.  Waarom is lege input bij voornaam dit keer WEL aanvaardbaar (i.t.t. bij het toevoegen)?

2.  Hoe wordt er omgegaan met lege input voor het aantal kinderen en waarom is -1 ok in dit geval?

3.  Wat zou je doen als ALLE mogelijke `int` waarden geldig zouden zijn om 'lege input' aan te duiden?

4.  Hoe weet het programma of de voornaam moet overschreven worden?

5.  Hoe weet het programma of het aantal kinderen moet overschreven worden?

### Oefening DPA1.verwijderen
Deze oefening gaat over de menu keuze "een persoon verwijderen".

Bekijk aandachtig de code van dit gedeelte en de gebruikte variabelen.

1.  Hoe gaat het verwijderen in z’n werk?

2.  Wat doen al die array manipulaties in de loop eigenlijk?

3.  Waarom moeten er in de arrays geen waarden op `0`/`false`/`null` gezet worden bij het verwijderen?

### Oefening DPA1.varia
Tot slot nog enkele losse inzichtsvragen :

1.  Hoe kun je (als gebruiker) het programma in zo’n toestand krijgen dat de gebruiker onmogelijk nog input kan geven die door het programma aanvaard wordt?

2.  Hoe kun je het programma laten crashen met een `IndexOutOfRangeException`?

3.  Wat vind je van de scope van de variabele isCorrectAntwoord, is die te ruim of net goed?

    -   Indien te ruim, hoe zou je de scope dan (beperkter) kiezen?

    -   Indien net goed, waarom mag de scope niet beperkter zijn?

4.  Is de variabele aantalPersonen vermijdbaar?

    -   Indien wel, hoe zou je de code aanpassen zodat deze overbodig wordt? (de aanpak uitleggen, je hoeft het NIET in de code aan te passen)

    -   Indien niet, waarom niet?

5.  Kan het verwijderen van een persoon gerealiseerd worden zonder al die array verschuivingen?

    -   Indien wel, hoe zou je de code aanpassen zodat deze overbodig wordt? (de aanpak uitleggen, je hoeft het NIET in de code aan te passen)

    -   Indien niet, waarom niet?

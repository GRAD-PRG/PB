# Programmeren Basis - Deel PA2 - Oefeningen
## Oefening DPA2.metmethods
Herwerk de broncode van het startpunt van de personenapplicatie zodat met onderstaande methods gewerkt wordt.

> **Tip**
>
> Bijna al deze methods zijn potentieel herbruikbaar in andere programma’s, vandaar dat we ze soms iets algemener maken dan strikt noodzakelijk voor de personenapplicatie.

Je zult de code in die methods moeten schrijven en op de juiste plaatsen in de `Main` method deze nieuwe methods oproepen.

Je zult zien dat je hierdoor veel code uit de `Main` methods zult kunnen weggooien en dat de `Main` method in het eindresultaat veel overzichtelijker is.

Werk deze oefening *method per method* uit, anders wordt het heel snel te verwarrend.

Begin dus met de eerste method toe te voegen en doe dan de nodige aanpassingen in de `Main` method zodat deze de eerste method gebruikt wordt.

Pas als dat allemaal werkt, begin je aan de tweede method enz.

### ToonTitel
`static void ToonTitel(string titel)`

Deze method toont de meegegeven titel op de console met een lijntje eronder. Het lijntje bestaat uit `-` symbolen en is precies even lang is als de titel tekst.

Dit fragment :

```csharp
ToonTitel("Hello World!");
Console.WriteLine("klaar");
```

produceert deze tekst op de console :

```csharp
Hello World!
------------
klaar
```

### VraagStringAntwoord
`static string VraagStringAntwoord(string vraag)`

Deze method stelt de opgegeven vraag aan de gebruiker en retourneert het ingetypte antwoord.

Dit fragment :

```csharp
string antwoord = VraagStringAntwoord("Wat is uw naam");
Console.WriteLine(antwoord);
```

heeft de volgende uitvoering als de gebruiker `Jan` invoert :

```csharp
Wat is uw naam : Jan
Jan
```

### VraagNietLeegStringAntwoord
`static string VraagNietLeegStringAntwoord(string vraag)`

Deze method stelt de opgegeven vraag aan de gebruiker en retourneert het ingetypte antwoord. Indien de gebruiker een antwoord geeft dat leeg is (of enkel whitespace bevat), blijft de method de vraag herhalen.

Dit fragment :

```csharp
string antwoord = VraagNietLeegStringAntwoord("Wat is uw naam");
Console.WriteLine(antwoord);
```

heeft de volgende uitvoering als de gebruiker eerst gewoon op Enter drukt, dan drie spaties typt en op Enter drukt en tenslotte `Jan` invoert en op Enter drukt.

```csharp
Wat is uw naam :
Wat is uw naam :
Wat is uw naam : Jan
Jan
```

### VraagKeuze
`static string VraagKeuze(string vraag, string[] keuzes)`

Deze method vraagt de gebruiker een keuze te maken uit de opgegeven keuzeteksten. Het toont de vraag en de mogelijke keuzes en laat de gebruiker iets invoeren. Indien de gebruiker niet één van de gegeven keuzes invoert, stelt het programma de vraag opnieuw. De invoer is hoofdletter**on**gevoelig.

**De method geeft altijd een tekst uit het meegegeven array terug**. Dit is niet noodzakelijk de tekst die de gebruiker intypte, de method is immers hoofdletterongevoelig.

Dit fragment :

```csharp
string[] kleuren = { "rood", "groen", "blauw", "geel" };
string antwoord = VraagKeuze("Welke kleur wenst u", kleuren);
Console.WriteLine($"U koos {antwoord}");
```

heeft de volgende uitvoering als de gebruiker eerst `paars` ingeeft, dan gewoon op Enter drukt zonder iets in te geven en tenslotte `BLaUw` invoert.

```csharp
Welke kleur wenst u (rood/groen/blauw/geel) : paars
Welke kleur wenst u (rood/groen/blauw/geel) :
Welke kleur wenst u (rood/groen/blauw/geel) : BLaUw
U koos blauw
```

> **Belangrijk**
>
> Let erop dat de geretourneerde tekst `blauw` is (uit het `kleuren` array) en niet de letterlijke `BLaUw` tekst die de gebruiker intypte.

### VraagIntAntwoord
`static int VraagIntAntwoord(string vraag, int min, int max)`

Deze method stelt de opgegeven vraag aan de gebruiker en aanvaardt enkel een leeg antwoord *of* een getal van `min` t.e.m. `max` (beiden inclusief).

De method zal de vraag herhalen totdat de gebruiker een aanvaardbaar antwoord geeft.

De return value van deze method is een `int` waarde die

-   ofwel `>= min` en `<= max` is

    -   als de gebruiker een getal binnen die grenzen invoerde

-   of `== int.MinValue`

    -   als de gebruiker een leeg antwoord gaf

Kort gezegd, ofwel krijg je een getal binnen de grenzen ofwel die speciale `int.MinValue` waarde.

De waarde `int.MinValue` is het kleinst mogelijke getal dat het `int` datatype kan voorstellen. De precieze waarde is hier eigenlijk niet van belang, maar mocht je nieuwsgierig zijn : het is `-2147483648`.

We reserveren hier dus één van de `int` waarden als speciaal geval, om aan te duiden dat de input leeg was. Iets vergelijkbaars zagen we al eens eerder : namelijk bij de verschillende `IndexOf` methods, daar werd telkens de `-1` waarde gebruikt als speciaal geval als er niks gevonden werd.

Voor `VraagIntAntwoord()` zou `-1` als speciale return value niet zo’n goeie keuze zijn : deze herbruikbare input method en heeft veel (potentiële) toepassingen waar `-1` een zinvol antwoord zou zijn. Wat we hier nodig hebben is een getal dat zo goed als nooit door eindgebruikers zal ingevoerd worden, bv. `int.MinValue`.

Dit fragment :

```csharp
int antwoord = VraagIntAntwoord("Hoe oud bent u", 0, 99);
Console.WriteLine($"U bent dus {antwoord} jaar oud");
```

heeft de volgende uitvoering als de gebruiker eerst `paars`, dan `-10`, dan `110` en tenslotte `25` invoert.

```csharp
Hoe oud bent u : paars
Hoe oud bent u : -1
Hoe oud bent u : 110
Hoe oud bent u : 25
U ben dus 25 jaar oud
```

Merk op dat de antwoorden `0` en `99` ook zouden aanvaard worden (wegens grenzen inclusief).

Indien de gebruiker een leeg antwoord geeft (of enkel whitespace, bv. 3 spaties) ziet de uitvoering er zo uit :

```csharp
Hoe oud bent u :
U ben dus -2147483648 jaar oud
```

Dit lijkt op zich niet zo zinvol, maar we kunnen makkelijk testen of de gebruiker al dan niet een getal invoerde!

```csharp
int antwoord = VraagIntAntwoord("Hoe oud bent u", 0, 99);
if (antwoord == int.MinValue) { // (1)
    Console.WriteLine("Oei, uw leeftijd is blijkbaar een gevoelig onderwerp");
} else {
    Console.WriteLine($"U bent dus {antwoord} jaar oud");
}
```

1.  Hier testen we op het speciale geval `int.MinValue`.

### VraagNietLeegIntAntwoord
`static int VraagNietLeegIntAntwoord(string vraag, int min, int max)`

Deze method doet hetzelfde als `VraagIntAntwoord` maar aanvaardt geen leeg antwoord. Indien de gebruiker een leeg antwoord (of enkel whitespace) invoert (of een getal dat buiten de grenzen ligt), dan herhaalt het programma de vraag.

> **Tip**
>
> Als je het slim aanpakt kun je in je `VraagNietLeegIntAntwoord` method gewoon `VraagIntAntwoord` oproepen (mits wat extra code errond).

Dit fragment

```csharp
int antwoord = VraagIntAntwoord("Hoe oud bent u", 0, 99);
Console.WriteLine($"U bent dus {antwoord} jaar oud");
```

heeft de volgende uitvoering als de gebruiker eerst `paars`, dan `-10`, dan een lege antwoord en tenslotte `25` invoert.

```csharp
Hoe oud bent u : paars
Hoe oud bent u : -1
Hoe oud bent u :
Hoe oud bent u : 25
U ben dus 25 jaar oud
```

### ToonPersoonDetails
`static void ToonPersoonDetails(int index, string[] voornamen, string[] familienamen, bool[] isVrouwen, string[] postcodes, string[] gemeenten, int[] aantalKinderen)`

Deze method toont de persoonsgegevens uit de parallelle arrays op positie `index`. De code van deze method mag ervan uitgaan dat `index` een geldige positie is in de array, er is geen controle nodig.

Dit fragment :

```csharp
ToonPersoonDetails(1, voornamen, familienamen, isVrouwen, postcodes, gemeenten, aantalKinderen);
```

produceert de volgende output als we de parallelle arrays uit de `Main` method veronderstellen met

-   op positie `0` de data voor "Jan Janssens"

-   op positie `1` de data voor "Mieke Mickelsen"

```csharp
voornaam    : Mieke
familienaam : Mickelsen
geslacht    : vrouw
postcode    : 9000
gemeente    : Gent
kinderen    : 0
```

De output is in dit geval gebaseerd op de waarden op positie `1` in de verschillende arrays.

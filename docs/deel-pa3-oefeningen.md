# Programmeren Basis - Deel PA3 - Oefeningen
## Oefening DPA3.metfileio
Herwerk [de oplossing DPA2.metmethods](../deel-pa2-oplossingen/deel-pa2-oplossingen.html#_oplossing_dpa2metmethods) zodat de persoonsgegevens in een bestand worden bewaard (en ingeladen bij de start).

We kiezen ervoor om een tekstbestand te gebruiken met naam `personen.csv`, waarin de waarden gescheiden worden door het tab-symbool i.p.v. komma’s.

In dit bestand staat de data voor elke persoon op een aparte regel. Per regel worden de verschillende waarden gescheiden door een tab-symbool. Ter herinnering, een tab-karakter in een string literal schrijf je als `"\t"`.

De locatie waar dit bestand moet staan is de (MS-Windows specifieke) folder `"My Documents"`.

In de broncode zul je het bestand moeten openen voor lezen/schrijven, je kunt daarbij dit fragment inlassen :

```csharp
string padNaarMyDocuments = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string padNaarFile = Path.Combine(padNaarMyDocuments, "personen.csv");
```

De variabele `padNaarFile` kun je dan gebruiken om het bestand te openen.

Denk eraan dat je helemaal bovenaan je broncode bestand, nog het volgende zult moeten toevoegen :

```csharp
using System.IO;
```

**Schrijf een method `LaadPersonen` die alle personen inleest en hun waarden in de verschillende arrays stopt.**

Al deze arrays zul je moeten meegeven als parameters en je mag ervan uitgaan dat deze arrays leeg zullen zijn.

```csharp
       (1)
static int LaadPersonen(string[] voornamen, string[] familienamen, bool[] isVrouwen, string[] postcodes, string[] gemeenten, int[] aantalKinderen) {
    // TODO : code toevoegen
}
```

1.  De teruggeefwaarde duidt op het aantal personen dat werd toegevoegd in de arrays.

Indien er geen enkele persoon kon ingelezen worden uit het bestand, dan stopt je programma de twee voorgedefinieerde personen (`Jan` en `Mieke`) in de arrays.

Indien er bij het lezen/verwerken/schrijven van een regel iets fout gaat, toont je programma een foutmelding en doet daarna verder met de rest.

Gebruik deze method voor de foutmeldingen :

```csharp
static void ToonFoutmelding(string bericht) {
    Console.WriteLine($"FOUT : {bericht}");
    Console.WriteLine("Druk op <Enter> om verder te gaan");
    Console.ReadLine();
}
```

Er zijn een aantal zaken die verkeerd kunnen gaan bij het inlezen

-   waarde voor isVrouw kan niet herkend worden als boolean (`bool.TryParse` herkent de waarde niet)

-   waarde voor aantal kinderen kan niet herkend worden als `int` (`int.TryParse` herkent de waarde niet)

-   regel bevat te weinig waarden (kun je makkelijk tellen, we verwachten 6 waarden per regel)

-   er is "iets anders" misgelopen zoals "bestand niet gevonden" of een "leesfout" (er komt een *exception* in dit geval)

**Schrijf een method `BewaarPersonen` die alle personen uit de arrays bewaart in het bestand.**

Dit overschrijft altijd alle informatie in het bestand, je hoeft niks toe te voegen of aan te passen in het bestand.

We schrijven dus telkens alle personen uit de arrays weg in het bestand en er blijft geen "oude informatie van een vorige keer" over in het bestand.

Al deze arrays zul je moeten meegeven als parameters.

```csharp
                                 (1)
static void BewaarPersonen(int aantalPersonen, string[] voornamen, string[] familienamen, bool[] isVrouwen, string[] postcodes, string[] gemeenten, int[] aantalKinderen) {
    // TODO : code toevoegen
}
```

1.  deze parameter dient om aan te geven hoeveel personen er in de arrays zitten (zodat je weet hoeveel *slots* je moet overlopen).

Indien het bestand nog niet bestaat, dan moet dit door je programma worden aangemaakt.

Indien er bij het bewaren iets verkeerd loopt toont het programma eveneens een foutmelding.

Bij het bewaren kan er niet zo veel mislopen :

-   er is iets misgelopen zoals "bestand niet gevonden" of een "schrijffout" (er komt een *exception* in dit geval)

# Werken met branches in Git

## Wat is een branch?

Een branch (vertakking) is een aparte lijn in de geschiedenis van je project. Je kunt het vergelijken met een kopie van je project waarop je verder werkt, zonder dat het origineel verandert.

Tot nu toe heb je altijd op de **main** branch gewerkt. Dat is de standaard branch die aangemaakt wordt bij elke nieuwe repository. Elke commit die je maakte, kwam op die ene lijn terecht:

```
main:  ●───●───●───●
```

Met branches kun je op een bepaald punt een aftakking maken. Je werkt dan verder op die nieuwe branch, terwijl `main` blijft staan waar je hem achterliet:

```
main:    ●───●───●
                  \
fase-2:            ●───●───●
```

De commits op `fase-2` bestaan alleen op die branch. Als je terugschakelt naar `main`, zie je de oude staat van je project — alsof je recente werk niet bestaat.

## Waarom branches?

In deze opdracht werk je in fases. Na elke bespreking met je lector krijg je feedback en nieuwe vereisten. Het is handig om op dat moment een duidelijke scheidslijn te trekken:

- **main** = de staat van je project bij de laatste bespreking (je laatst goedgekeurde tussenresultaat)
- **een nieuwe branch** = al het werk dat je doet vóór de volgende bespreking

Zo kan je lector bij de volgende bespreking makkelijk vergelijken: wat stond er op `main` (het vorige tussenresultaat), en wat heb je sindsdien veranderd (de branch)?

Die vergelijking gebeurt via een **Pull Request** op GitHub — daar komen we straks op terug.

## Een branch aanmaken in Visual Studio

> Je maakt een nieuwe branch aan **na** een bespreking, wanneer je klaar bent om aan de volgende fase te beginnen.

1. Kijk onderaan in de statusbalk van Visual Studio. Daar staat de naam van je huidige branch, waarschijnlijk **main**.

2. Klik op die naam. Er verschijnt een menu.

3. Kies **New Branch...** (of **Nieuwe vertakking...**).

4. Vul een naam in voor je branch, bijvoorbeeld `fase-2`. Gebruik geen spaties of speciale tekens.

5. Bij **Based on** staat normaal `main` — laat dat zo staan.

6. Zorg dat **Checkout branch** aangevinkt is. Dit zorgt ervoor dat je meteen op de nieuwe branch terechtkomt.

7. Klik **Create**.

Onderaan in de statusbalk staat nu de naam van je nieuwe branch in plaats van `main`. Alle commits die je vanaf nu maakt, komen op deze branch terecht.

## Werken op je branch

Je werkt op je branch precies zoals je gewend bent:

- Code schrijven en aanpassen
- Commits maken met een duidelijke boodschap
- Pushen naar GitHub

Bij de eerste push zal Visual Studio de branch ook aanmaken op GitHub. Dit gebeurt automatisch.

> **Belangrijk:** schakel niet terug naar `main` zolang je aan het werk bent. Al je wijzigingen moeten op de branch terechtkomen.

## Een Pull Request aanmaken

Een Pull Request (PR) is een verzoek op GitHub om de wijzigingen van je branch samen te voegen met `main`. Maar het is ook — en voor ons vooral — een handige manier om al je wijzigingen in één overzicht te bekijken.

> Maak je Pull Request aan **vóór** de volgende bespreking, nadat je al je werk hebt gecommit en gepusht.

1. Ga naar je repository op [github.com](https://github.com).

2. GitHub toont meestal een gele banner bovenaan:
   > **fase-2** had recent pushes — **Compare & pull request**

   Klik op die groene knop.

   Zie je de banner niet? Ga dan naar het tabblad **Pull requests** en klik op **New pull request**. Kies `base: main` en `compare: fase-2`.

3. Geef de Pull Request een titel, bijvoorbeeld "Fase 2: wijzigingen na bespreking 1".

4. Klik op **Create pull request**.

### Wat zie je in een Pull Request?

De PR heeft een aantal tabbladen. Het belangrijkste is **Files changed**. Daar zie je voor elk bestand wat er veranderd is ten opzichte van `main`:

- Groene regels (+) zijn **toegevoegd**.
- Rode regels (−) zijn **verwijderd**.
- Ongewijzigde regels staan er grijs bij voor context.

Dit is het overzicht dat je lector gebruikt om je werk te beoordelen. Je lector kan ook **commentaar achterlaten op specifieke regels** door op het blauwe **+**-icoon naast een regel te klikken.

## Issues koppelen aan een Pull Request

Issues en branches staan los van elkaar: een issue is een taak of opmerking, een branch is een lijn in je codegeschiedenis. Maar je kunt ze aan elkaar koppelen.

Als je in de beschrijving van je Pull Request schrijft:

```
Fixes #3
Closes #5
```

dan sluit GitHub die issues automatisch wanneer de PR gemerged wordt. Zo houd je bij welke taken afgewerkt zijn.

> Dit is optioneel maar handig — bespreek met je lector of en hoe je dit toepast.

## Mergen

Na de bespreking, als je lector akkoord geeft, wordt de branch gemerged met `main`. Dat betekent dat alle wijzigingen van je branch samengevoegd worden met `main`, zodat `main` weer up-to-date is.

```
main:    ●───●───●───────────●  (merge commit)
                  \         /
fase-2:            ●───●───●
```

> Het mergen gebeurt samen met je lector op GitHub, via de knop **Merge pull request** onderaan de PR.

### Na de merge: terug naar main

Na het mergen moet je in Visual Studio terugschakelen naar `main` en je lokale kopie bijwerken:

1. Klik onderaan in de statusbalk op de naam van je branch (bv. `fase-2`).
2. Kies **main** uit de lijst.
3. Doe een **Pull** (via **Git → Pull** in het menu, of het pijltje-omlaag in de statusbalk).

Nu is je lokale `main` weer up-to-date met alles wat je op de branch gedaan hebt. Vanaf hier kun je een nieuwe branch aanmaken voor de volgende fase, en begint de cyclus opnieuw.

## Samenvatting van de cyclus

| Stap | Wat | Waar |
|---|---|---|
| Bespreking | Tussenresultaat bespreken, issues aanmaken | In de klas |
| Branch aanmaken | Nieuwe branch vanuit `main` | Visual Studio |
| Werken | Code schrijven, committen, pushen | Visual Studio |
| PR aanmaken | Pull Request openen naar `main` | GitHub |
| Volgende bespreking | Code review via de PR | In de klas + GitHub |
| Merge | Branch samenvoegen met `main` | GitHub |
| Opruimen | Terugschakelen naar `main`, pullen | Visual Studio |

## Veelgemaakte fouten

**Vergeten van branch te wisselen na de merge.** Als je na het mergen op GitHub gewoon verder werkt in Visual Studio, zit je nog op de oude branch. Schakel altijd eerst terug naar `main` en pull.

**Per ongeluk op main werken.** Controleer altijd onderaan in de statusbalk op welke branch je zit vóór je begint te coderen. Staat er `main` terwijl je op een branch zou moeten zitten? Maak dan eerst je branch aan.

**Pushen vergeten vóór de bespreking.** Je lector kan alleen code zien die op GitHub staat. Lokale commits die niet gepusht zijn, zijn onzichtbaar. Push altijd vóór je naar een bespreking komt.

---

## Verder: handige extras

De stappen hierboven zijn alles wat je nodig hebt om met branches te werken. Wat hieronder volgt zijn optionele technieken die het proces nog vlotter kunnen maken.

### Een branch aanmaken vanuit een issue op GitHub

In plaats van eerst een branch aan te maken in Visual Studio en dan pas te beginnen werken, kun je ook vertrekken vanuit een issue op GitHub. Dit koppelt de branch meteen aan het issue.

1. Ga naar het issue op GitHub.

2. In de rechterzijbalk zie je een sectie **Development**. Klik op **Create a branch**.

3. GitHub stelt een naam voor op basis van het issuenummer en de titel, bijvoorbeeld `3-fix-validatie`. Je kunt die naam aanpassen.

4. Klik op **Create branch**.

5. GitHub toont je een commando om de branch lokaal op te halen. In Visual Studio kun je dit eenvoudiger doen: doe een **Fetch** (via **Git → Fetch**), klik daarna op de branchnaam onderaan in de statusbalk, en kies de nieuwe branch uit de lijst onder **Remote branches**.

Het voordeel: in de Pull Request die je later aanmaakt, verschijnt automatisch een link naar het issue. En als je de PR merget, wordt het issue automatisch gesloten — zonder dat je `Fixes #3` hoeft te schrijven.

### Een Pull Request aanmaken vanuit Visual Studio

Na het pushen van je branch toont Visual Studio een melding:

> Successfully pushed to origin/fase-2. Pull request: **Create in Visual Studio** — **Create in browser**

Als je op **Create in Visual Studio** klikt, opent er een formulier in VS zelf waar je de titel en beschrijving kunt invullen. Het resultaat is identiek aan een PR die je via de browser aanmaakt.

Dit is puur een kwestie van voorkeur. De browserversie van GitHub geeft je een iets rijkere interface (je ziet bijvoorbeeld meteen de diff), maar voor het aanmaken zelf maakt het geen verschil.

### Branch protection rules

Standaard kan iedereen rechtstreeks op `main` pushen. Dat is een risico: als je per ongeluk op `main` werkt in plaats van op je branch, overschrijf je het tussenresultaat dat als referentiepunt dient.

Je lector kan een **branch protection rule** instellen die dit voorkomt. Daarmee wordt afgedwongen dat wijzigingen aan `main` alleen via een Pull Request mogen gebeuren. Rechtstreeks pushen naar `main` wordt dan geblokkeerd.

Dit is iets wat je lector instelt, niet iets wat je zelf hoeft te doen. Maar als het actief is en je probeert te pushen naar `main`, krijg je een foutmelding. De oplossing is dan simpel: maak een branch aan en werk daar verder.

> **Voor lectoren:** de instelling vind je op GitHub via **Settings → Branches → Add branch ruleset**. Maak een regel aan voor `main` met als vereiste **Require a pull request before merging**. Optioneel kun je ook **Require approvals** aanvinken, zodat een PR pas gemerged kan worden nadat jij als reviewer hebt goedgekeurd.

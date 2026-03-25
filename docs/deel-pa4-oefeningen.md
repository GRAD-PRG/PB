# Programmeren Basis - Deel PA4 - Oefeningen
## Oefening DPA4.metpersoonklasse
**Om deze oefening te kunnen oplossen moet je het stuk over klassen en properties al doorgenomen hebben.**

Herwerk [de oplossing van DPA2.metmethods](../deel-pa2-oplossingen/deel-pa2-oplossingen.html#_oplossing_dpa2metmethods) zodat alle gegevens van één (fysieke) persoon in één (digitaal) persoon object wordt bijgehouden.

In plaats van de gegevens van een persoon losjes te verdelen over 6 parallelle arrays, bundelen we ze in `Persoon` objecten en houden al die objecten bij in één `Persoon[]` (i.e. een array van verwijzingen naar `Persoon` objecten).

Je zult hiervoor dus een klasse `Persoon` moeten toevoegen en `Persoon` objecten gebruiken in het programma.

Je zult op meerdere plaatsen in de `Main` method aanpassingen moeten doen.

Je zult ook de `ToonPersoonDetails` method moeten aanpassen zodat die één `Persoon` parameter heeft i.p.v. allemaal aparte parameters voor de persoonsgegevens.

# Popis dokumentu

Můj první PBI dokument vytvořený na základě zkušebních dat od aplikace.
Na prvním grafu vidíme výši výdělku dle kalendářního měsíce, na druhém obrázku si v mapě můžeme zobrazit výši výdělku v konkrétní zemi.
Na spodním grafu potom vidíme výši prodeje pro jednotlivé produkty. Každý produkt je barevně rozdělen do sloupcových grafů zaznamenávajících firmy, které ho vyrábějí.
Vlevo si můžeme rozkliknout konkrétní měsíc, který nás zajímá a grafy se přizpůsobí pouze pro zvolené období.

## Použité pomocné tabulky

### Calendar

Vytvořená pomocí:
```DAX
Calendar = CALENDAR(DATE(2013,01,01), DATE(2014,12,31))
```

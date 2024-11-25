# Kivételkezelés (exception handling)

Egy program működése közben különféle hibák adódhatnak (pl. nincs jogosultságunk egy fájl eléréséhez, valamilyen hiba történik a hálózati kommunikáció során, az aktuálisan bekért adatok nullával való osztást eredményeznek, stb.). 
Nem lenne szerencsés, ha ilyen esetekben a program működése egyszerűen leállna, különösen akkor nem, ha egy olyan rendszerről van szó (pl. egy banki rendszerről), amit sok felhasználó használ egyidejűleg. 
A hibákat tehát kezelni szeretnénk. A konkrét hibakezelés természetesen függ magától a hibától: lehet, hogy újra bekérjük az adatokat a felhasználótól, de az is lehet, hogy csak egy "értelmes" hibaüzenetet adunk, vagy valami mást csinálunk, pl. 
az egyik szerver helyett egy másikról próbáljuk meg letölteni ugyanazt a fájlt (feltéve, hogy tudjuk, hogy ugyanaz a fájl több helyen is elérhető).

Amikor hiba adódik, egy kivétel (exception) típusú objektum jön létre és ez lesz annak a metódusnak a visszatérési értéke, amiben a hiba adódott. Ezt a speciális visszatérési értéket nem szoktuk visszatérési értéknek nevezni, hanem 
azt mondjuk helyette, hogy a metódus egy kivétlet dob (throws an excpetion). Ha tehát egy M metódus végrehajtása során hiba keletkezik, akkor M dob egy kivételt, amit vagy "elkap" (catch) az M-t hívó metódus vagy pedig továbbdob.
Az M-et hívó metódust nevezzük most M'-nek. Ha M' továbbdobja a hibát, akkor annak a metódusnak dobja tovább, ami M'-t hívta. Az M'-t hívó metódus, hasonlóan M'-höz, vagy elkapja (azaz kezeli) a hibát vagy továbbdobja. És így tovább. 
Ha minden metódus csak továbbdobja a hibát, akkor az végül a java virtuális géphez kerül, ami a hiba miatt leállítja a program futását, miközben egy hibaüzenetet ír ki az ún. standard error kimeneten. Ha parancssorból indítjuk a Java 
programunkat, akkor a standard error-ra kiírt hibaüzenet általában ugyanabban a parancssori ablakban jelenik meg, ahol a standard output is megjelent (persze mindkét kimenetet, azaz a a standard output-ot és a standard error-t is,
átirányíthatjuk valahova máshova, de ezzel most nem foglalkozunk...) 

A kovetkezőekben egy példán keresztül mutatjuk be a kivételek kezelését. 

# 0. lépés: téglalapok kivételkezelés nélkül

Tekintsük az alábbi, téglalapokat reprezentáló osztályt.

Teglalap.java:

```
Kiindulunk a korábbi Teglalap osztályunkból:

public class Teglalap {
    private double a;
    private double b;

    public Teglalap(double a, double b) {
        this.a = a; 
        this.b = b;
    }

    public double kerulet() {
        return 2*a+2*b;
    }

    public double terulet() {
        return a*b;
    }
}
```

# 1. lépés: kivétel dobása negatív oldalhossz esetén

Tételezzük fel, hogy olyan alkalmazás számára végzünk szoftvrefejlesztést, amiben nincs értelme a negatív oldalhosszúságú téglalapoknak, ezért ha valaki negatív oldalhosszúságú téglalapot hozna létre, kivételt dobunk:

Teglalap.java:

```
public class Teglalap {
    private double a;
    private double b;

    public Teglalap(double a, double b) throws Exception {
        if ((a < 0) || (b<0)) {
            throw new Exception("Egy teglalap oldalanak hossza nem lehet negativ.");
        }
        this.a = a;
        this.b = b;
    }

    public double kerulet() {
        return 2*a+2*b;
    }

    public double terulet() {
        return a*b;
    }
}
```

Nézzük meg, mi történik, ha negatív oldalhosszúságú téglalapot próbálunk létrehozni. 
Mivel a Teglalap osztály konstruktora hibát dobhat, ezt vagy elkapjuk...

Main.java:

```
public class Main {
    public static void main(String[] args) {
        try {
            new Teglalap(-2, 5);
            System.out.println("Ez csak akkor hajtódik vegre, ha a " +
              "teglalap letrehozasa kozben nem dobott exceptiont");
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

...vagy továbbdobjuk a java virtuális gépnek:

Main.java:

```
public class Main {
    public static void main(String[] args) throws Exception {
        new Teglalap(-2, 5);
        System.out.println("Ez csak akkor hajtódik vegre, ha a " +
          "teglalap letrehozasa kozben nem dobott exceptiont");
    }
}
```

Természetesen nem csak a konstruktor dobhat hibát, hanem bármilyen más metódus is. Példaként készítsük el a kerületszámoló metódus "válogatós" 
változatát, amely egyes téglalapok kerületét nem számolja ki: 

Teglalap.java:

```
public class Teglalap {

    ...

    public double kerulet() throws Exception {
        if (a == 314) {
            throw new Exception("Nem szeretem az olyan teglalapokat, amelyek" +
              "a-val jelolt oldala emlkeztet PI-re.");
        }
        return 2*a+2*b;
    }

    ...

}
```

Ennek persze semmi értelme sincsen (azon kívül persze, hogy illusztráljuk, hogy nem csak a konstruktor dobhat hibát), 
ezért amilyen gyorsan létrehoztuk ezt a metódust, olyan gyorsan el is felejtjük. 


FOLYT.KÖV!

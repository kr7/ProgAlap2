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

## 0. lépés: téglalapok kivételkezelés nélkül

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

## 1. lépés: kivétel dobása negatív oldalhossz esetén

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


## 2. lépés: hibakezelés

Ha alaposabban szemügyrevesszük a korábbi kódot, ahol "elkaptuk" a hibát, látható, hogy az a függvény, amelyben a hiba felléphet (amely a kivételt dobja) a try blokkba került, az ezt követő 
catch blokk pedig a hibát kezelő kódot tartalmazza. A példában a hibakezelés bármilyen Exception típusú kivételt kezel, a konkrét hibát reprezentáló objektumra az "e" referenciával hivatkozunk.
Nézzünk egy másik példát a hibakezelésre. Ebben az esetben, ha nem sikerült létrehozni a téglalapot, új adatokat kérünk be a felhasználótól:

Main.java:

```
import javax.swing.JFrame;
import javax.swing.JOptionPane;

public class Main {
    public static Teglalap felhasznaloiTeglalap() {
        JFrame frame = new JFrame("Teglalap adatai");
        String a_str = JOptionPane.showInputDialog(frame, "a: ");
        String b_str = JOptionPane.showInputDialog(frame, "b: ");
        double a = Double.parseDouble(a_str);
        double b = Double.parseDouble(b_str);
        try {
            return new Teglalap(a, b);
        } catch (Exception e) {
            JOptionPane.showMessageDialog(frame, e.getMessage());
            return felhasznaloiTeglalap();
        }
    }

    public static void main(String[] args) {
        Teglalap t = felhasznaloiTeglalap();
        System.out.println( t.terulet() );
    }
}
```

## 3. lépés: saját exception


De mi van, ha a téglalap létrehozása közben valamilyen más hiba adódik? Elképzelhetünk egy komolyabb alkalmazást, amiben a téglalap létrehozásakor le akarjuk menteni annak adatait akár egy fájlba, akár egy adatbázisszerverre. Ehhez fájlműveletet vagy hálózati műveletet szeretnénk végezni, és eközben előfordulhat, hogy nincs elég hely a lemezen, nem vagyunk jogosultak a fájlhoz való hozzáférésre vagy nincs internet kapcsolat. Ilyenkor újra bekérni a téglalap adatait nem a legjobb ötlet. Az "Exception" bármilyen hibát lefed, ezért az eddigi kódunk bármilyen hiba esetén újra bekérné a téglalap oldalainak hosszát a felhasználótól, akkor is ha fájlművelet vagy hálózati művelet miatt adódott a hiba.

A program javításához először definiálunk egy saját kivétel osztályt, amely az Exception (java.lang.Exception) leszármaztatottja lesz:

TeglalapException.java:

```
public class TeglalapException extends Exception {
    public TeglalapException(String message) {
        super(message);
    }
}
```

A Teglalap konstruktora pedig a saját kivételosztályunk egy példányát fogja dobni:

Teglalap.java:

```
public class Teglalap {
    private double a;
    private double b;

    public Teglalap(double a, double b) throws TeglalapException {
        if ((a < 0) || (b<0)) {
            throw new TeglalapException("Egy teglalap oldalanak hossza nem lehet negativ.");
        }
        this.a = a;
        this.b = b;
    }

    ...

}  
```

A főprogramban pedig ezt fogjuk elkapni és kezelni. Ha valamilyen más hiba adódna akkor - mivel más hibát nem kezelünk - azt továbbdobnánk a java virtuális gépnek, és emiatt hibaüzenettel leállna a programunk (de legalább nem kérnénk be újra a téglalap oldalhosszait, ha egyszer az oldalhosszak rendben vannak):

Main.java:

```
import javax.swing.JFrame;
import javax.swing.JOptionPane;

public class Main {
    public static Teglalap felhasznaloiTeglalap() {
        JFrame frame = new JFrame("Teglalap adatai");
        String a_str = JOptionPane.showInputDialog(frame, "a: ");
        String b_str = JOptionPane.showInputDialog(frame, "b: ");
        double a = Double.parseDouble(a_str);
        double b = Double.parseDouble(b_str);
        try {
            return new Teglalap(a, b);
        } catch (TeglalapException e) {
            JOptionPane.showMessageDialog(frame, e.getMessage());
            return felhasznaloiTeglalap();
        }
    }

    public static void main(String[] args) {
        Teglalap t = felhasznaloiTeglalap();
        System.out.println( t.terulet() );
    }
}
```

## 4. lépés: a különböző hibákat más-más módon kezeljük

Az előbbiekből már látszik, hogy érdemes különböző hibákat más-más módon kezelnünk. Ennek szemléltetéséhez definiálunk egy háromszögeket reprezentáló osztályt, amely 
több okból is dobhat kivételt:

Haromszog.java:

```
public class Haromszog {
    private double a;
    private double b;
    private double c;

    public Haromszog(double a, double b, double c) throws HaromszogException{
        if ((a<0) || (b<0) || (c<0)) {
            throw new HaromszogException("Egy haromszog egyik oldala sem lehet negativ hosszusagu");
        }
        if ( (a+b<c) || (b+c<a) || (a+c<b) ) {
            throw new HaromszogException("Nem teljesul a haromszogegyenlotlenseg");
        }
        this.a=a;
        this.b=b;
        this.c=c;
    }
}
```

Az előbbi osztályban használt kivételt reprezentáló osztály:

HaromszogException.java:

```
public class HaromszogException extends Exception {
    public HaromszogException(String message) {
        super(message);
    }
}
```

Ahhoz, hogy a két különböző okból dobott kivételt különböző módon kezeljük a catch blockban, a két különböző kivétel számára két külön osztályt hozunk létre: 

HaromszogOldalaNemLehetNegativException.java:

```
public class HaromszogOldalaNemLehetNegativException extends HaromszogException {
    public HaromszogOldalaNemLehetNegativException(String message) {
        super(message);
    }
}
```

HaromszogegyenlotlensegNemTeljesulException.java:

```
public class HaromszogegyenlotlensegNemTeljesulException extends HaromszogException {
    public HaromszogegyenlotlensegNemTeljesulException(String message) {
        super(message);
    }
}
```

Vegyük észre, hogy a kivételek hierarchikus struktúrát alkotnak: mindkét fenti kivétel a HaromszogException leszármazottja, maga a HaromszogException pedig az Exception leszármazottja. Más szavakkal: minden HaromszogOldalaNemLehetNegativException egyben egy HaromszogException is, de nem minden HaromszogException-re igaz, hogy HaromszogOldalaNemLehetNegativException, mert egy HaromszogException lehet HaromszogegyenlotlensegNemTeljesulException is. Ezért a catch blockban egy HaromszogException mind a kétféle hibát "elkapja", a 
HaromszogOldalaNemLehetNegativException viszont csak azt, amikor olyan háromszöget próbálunk létrehozni, aminek egyik oldala negatív hosszúságú.

A Haroszog.java az alábbiak szerint módosul:

Haromszog.java:

```
public class Haromszog {
    private double a;
    private double b;
    private double c;

    public Haromszog(double a, double b, double c) throws HaromszogException{
        if ((a<0) || (b<0) || (c<0)) {
            throw new HaromszogOldalaNemLehetNegativException("Egy haromszog egyik oldala sem lehet negativ hosszusagu");
        }
        if ( (a+b<c) || (b+c<a) || (a+c<b) ) {
            throw new HaromszogegyenlotlensegNemTeljesulException("Nem teljesul a haromszogegyenlotlenseg");
        }
        this.a=a;
        this.b=b;
        this.c=c;
    }

    public double kerulet() {
        return a+b+c;
    }
}
```


Main.java:

```
import javax.swing.JFrame;
import javax.swing.JOptionPane;

public class Main {
    public static Haromszog felhasznaloiHaromszog() {
        JFrame frame = new JFrame("Haromszog oldalai");
        String a_str = JOptionPane.showInputDialog(frame, "a: ");
        String b_str = JOptionPane.showInputDialog(frame, "b: ");
        String c_str = JOptionPane.showInputDialog(frame, "c: ");
        double a = Double.parseDouble(a_str);
        double b = Double.parseDouble(b_str);
        double c = Double.parseDouble(c_str);
        try {
            return new Haromszog(a, b, c);
        } catch (HaromszogOldalaNemLehetNegativException e) {
            JOptionPane.showMessageDialog(frame, e.getMessage());
            return felhasznaloiHaromszog();
        } catch (HaromszogegyenlotlensegNemTeljesulException e) {
            // Ha nem teljesul a haromszogegyenlotlenseg, akkor nem kerunk be uj adatokat,
            // hanem csak visszaadunk egy egyenlo oldalu haromszoget.
            // Lehet, hogy ez egy valos alkalmazasban nem lenne a legjobb otlet,
            // ennek pusztan az a celja, hogy demonstraljuk, hogy a kulonbozo
            // hibakat maskent kezeljuk.
            try { 
                return new Haromszog(1,1,1);
            } catch (Exception e1) {
                // nem csinalunk semmit, mert a konstans 1,1,1 oldalhosszusagu haromszog 
                // letrehozasakor nem adodhat semmilyen hiba ebben a peldaban, tehat
                // ezt a blokkot igazabol soha nem fogjuk vegrehajtani
                return null;
            }
        } catch (HaromszogException e) {
            // a Haromszog konstruktora egy HaromszogException tipusu kivetelt dob, es elvileg 
            // a letezhetne  elobbin kivul tovabbi altipusa is a HaromszogException-nek, vagy
            // lehet, hogy a kod fejlesztese soran valaki majd definial egy ilyet, ezert
            // "altalaban" is el kell kapnunk a HaromszogException-oket.
            // Ha egy exceptiont a korabbi blokkok valamelyike mar elkapott, akkor ez a blokk
            // nem hajtodik vegre.
            System.out.println(e);
            return null;
        }
    }

    public static void main(String[] args) {
        h = felhasznaloiHaromszog();
        System.out.println( h.kerulet() );
    }
}
```

## 5. lépés: záró megjegyzés

Megjegyzés: a kivételeket reprezentáló osztályokat egy ún. package-be szervezhetjük. Ezt úgy 
képzelhetjük el, hogy ezeket az osztályokat betesszük egy mappába azért, hogy átláthatóbb 
legyen a kódunk. 

# Objektumorientált programozás 

Az objektumorientált programozás legfontosabb fogalmait egy példán keresztül mutatjuk be lépésről lépésre.
Konkrétan síkidomok kerületét és területét fogjuk számolni Java-ban írt kód segítségével.

## 0. lépés: terület és kerületszámítás 

Kiszámoljuk egy 10 és 20 hosszúságú oldalakkal rendelkező téglalap, valamint egy 3 és 4 hosszú befogókkal rendelkező derékszögű háromszög kerületét és területét.

Main.java:

```
public class Main {
    public static void main(String[] args) {
        // Egy a=10 és b=20 oldalhosszusagu teglalap kerulete es terulete
        double a = 10;
        double b = 20;
        double k = 2*a+2*b;
        double t = a*b;

        // Egy a_h=3 es b_h=4 befogokkal rendelkezo derekszogu haromszog
        // kerulete es terulete
        double a_h = 3.0;
        double b_h = 4.0;
        double c_h = Math.sqrt( Math.pow(a_h,2) + Math.pow(b_h,2) ); // ld. Pitagorasz-tétel 
        double k_h = a_h+b_h+c_h;
        double t_h = a_h*b_h/2;

        System.out.println("A teglalap kerulete: "+k+", terulete: "+t);
        System.out.println("A derekszogu haromszog kerulete: "+k_h+", terulete: "+t_h);
    }
}
```

## 1. lépés: függvények 

Miközben az előbbi kód helyes eredményeket számol, a feladat ilyen megoldása sok szempontból sem praktikus. 
Ha például nem csak egy háromszög kerületét szeretnénk kiszámolni, hanem több háromszöggel is dolgozunk, akkor
a kerületszámítást végző kódot (az előbbi minta szerint) többször is tartalmazná a programunk. És ha valaki 
"feltalálja" a kerületszámítás új, hatékony módját ;-) , akkor több ponton is változtatnunk kellene a kódunkat.
Vagy ha kiderült, hogy a kerületszámítási eljárásunk hibás (bug-os), szintén több helyen kellene átírnunk a kódot. 
Egy minimalisztikus példában ez persze nem gond, de gondoljunk komolyabb szoftverprojektekre: lehet, hogy sokkal
bonyolúltabb számításokat végzünk a kerületszámításnál, és a teljes kódbázis sokezer kódsorból állhat. 
Ilyen esetekben általában nem lenne könnyű megtalálni az összes olyan helyet, ahol változtatnunk kellene, ezért
fennállna a veszély, hogy
elfelejtenénk valahol változtatni a kódot, vagy valahol hibásan végeznénk el a változtatást.

A programuk "javítása" érdekében első lépésként a kerület és területszámításokat külön függvénybe szervezzük.

Main.java:

```
public class Main {

    public static double teglalap_kerulete(double a, double b) {
        return 2*a+2*b;
    }

    public static double teglalap_terulete(double a, double b) {
        return a*b;
    }

    public static double derekszogu_haromszog_kerulete(double a, double b) {
        double c = Math.sqrt( Math.pow(a,2) + Math.pow(b,2) );
        return a+b+c;
    }

    public static double derekszogu_haromszog_terulete(double a, double b) {
        return a*b/2;
    }

    public static void main(String[] args) {
        // Egy a=10 és b=20 oldalhosszusagu teglalap kerulete es terulete
        double a = 10;
        double b = 20;
        double k = teglalap_kerulete(a,b);
        double t = teglalap_terulete(a,b);

        // Egy a_h=3 es b_h=4 befogokkal rendelkezo derekszogu haromszog
        // kerulete es terulete
        double a_h = 3.0;
        double b_h = 4.0;
        double k_h = derekszogu_haromszog_kerulete(a_h, b_h);
        double t_h = derekszogu_haromszog_terulete(a_h, b_h);

        System.out.println("A teglalap kerulete: "+k+", terulete: "+t);
        System.out.println("A derekszogu haromszog kerulete: "+k_h+", terulete: "+t_h);
    }
}
```

## 2. lépés: osztályok defíniciója

Az előbbi megoldás mellett lehetséges, hogy a háromszög kerület- és területszámító függvényeit használjuk
a téglalap adataira, vagy éppen fordítva. Az objektum-orientált programozás alapgondolata, hogy az 
adatokat és a hozzájuk kapcsolódó függvényeket egy egységbe, ún. osztályba, szervezzük. Ezért defináljuk 
a Teglalap és DerekszoguHaromszog osztályokat a Teglalap.java illetve DerekszoguHaromszog.java elnevezésű
fájlokban. A főprogramban ezeket az osztályokat fogjuk példányosítani, más szóval: létrehozzuk a konkrét
derékszögű háromszögnek és a konkrét téglalapnak megfelelő objektumokat a **new** kulcsszó használatával. 
A példányosítás során adjuk meg azon téglalap és háromszög adatait (oldalhosszakat), amelyekkel ténylegesen dolgozunk a programban.

Ha több téglalapunk vagy több háromszögünk lenne, akkor többször példányosítanánk az osztályokat, azaz a 
new kulcsszó-t tartalmazó sorokhoz hasonló további sorokat szerepeltetnénk a programban). Ugyanakkor
az osztályok leírását, azaz a Teglalap.java illetve DerekszoguHaromszog.java elnevezésű
fájlokat elég lenne egyszer létrehoznunk. Ez logikus, hiszen ezek a fájlok általában írják le, hogy az épp
készülő programunkban egy téglalap illetve háromszög milyen adataival dolgozunk (jelen példában: 
csak az oldalhosszakkal), és azokkal az adatokkal
milyen műveleteket végzünk (a példánkban: kerület és területszámítást). 
A Teglalap.java illetve DerekszoguHaromszog.java elnevezésű
fájlok tehát egyfajta általános leírást
adnak a téglalapokról és a derékszögű háromszögekről abban az értelemben, hogy semmi olyan nem 
szerepel bennük, ami csak egy konkrét téglapra vagy csak egy konkrét derékszögű háromszögre lenne
igaz: nyilván egy más oldalhosszúságú
téglalapok és háromszögek esetében is ugyanolyan módon végezzük a terület és kerület számítását. 

Teglalap.java:

```
public class Teglalap {
    private double a;
    private double b;

    public Teglalap(double a, double b) {
        this.a = a; 
        this.b = b;
        // A "this" kulcsszón keresztül hivatkozunk az objektumpéldány
        // adattagjaira és metódusaira
    }

    public double kerulet() {
        return 2*a+2*b;
        // Ide nem kell "this", mert ennek a metódusnak nincs saját "a" és "b" változója,
        // ekkor az "a" és "b" az objektumpéldány "a" és "b" változóját jelenti.
    }

    public double terulet() {
        return a*b;
    }
}
```

DerekszoguHaromszog.java:

```
public class DerekszoguHaromszog {
    private double a;
    private double b;

    public DerekszoguHaromszog(double a, double b) {
        this.a=a;
        this.b=b;
    }

    public double kerulet() {
        double c = Math.sqrt( Math.pow(a,2) + Math.pow(b,2) );
        return a+b+c;
    }

    public double terulet() {
        return a*b/2;
    }
}
```

Main.java: 

```
public class Main {
    public static void main(String[] args) {
        // Egy a=10 és b=20 oldalhosszusagu teglalap kerulete es terulete
        Teglalap tgl = new Teglalap(10, 20);
        double k = tgl.kerulet();
        double t = tgl.terulet();

        // Egy a_h=3 es b_h=4 befogokkal rendelkezo derekszogu haromszog
        // kerulete es terulete
        DerekszoguHaromszog h = new DerekszoguHaromszog(3,4);
        double k_h = h.kerulet();
        double t_h = h.terulet();

        System.out.println("A teglalap kerulete: "+k+", terulete: "+t);
        System.out.println("A derekszogu haromszog kerulete: "+k_h+", terulete: "+t_h);
    }
}
```

Ha alaposabban szemügyre vesszük az osztályok leírását, látjuk, hogy az osztályokat leíró kód

- az osztályok által reprezentált téglalap illetve háromszög adatait tároló változók definíciójával (oldalhosszakat tároló "a" és "b" double-k) kezdődik, ezeket az osztályok **adattagjainak** nevezzük,
- majd egy "szokatlan" metódussal folytatódik, amelynek a neve megegyezik az osztály nevével,
- ezt követik a terület- és kerületszámító **metódusok**. (Az objektumorientált programozás kontextusában az egyes objektumok "függvényeit" metódusnak nevezzük.)

A "szokatlan" metódus az ún. konstruktor, az osztály példányosításakor ez kerül végrehajtásra. Az osztályokat elnevezésüknek megfelelő ".java" fájlokban tároljuk, azaz "Teglalap.java" ill. "DerekszoguHaromszog.java" fájlban  (ez lényeges!).

Az osztályok definíciója során megadtuk az osztályok adattagjainak és metódusainak **láthatóságát** (a példában **private** és **public**, de létezik **protected** és **default** láthatóság is, lásd: [https://www.w3schools.com/java/java_modifiers.asp](https://www.w3schools.com/java/java_modifiers.asp) ). 

## 3. lépés: setter és getter metódusok

Az osztályok adattagjait tipikusan private láthatósági szintűnek definiáljuk, ugyanis úgy tekintjük, hogy ez a téglalap illetve háromszög "magánügye". Az osztály "felhasználója" (egy programozó, aki használja az osztályt) az osztály metódusain keresztül kommunikál az objektumokkal, nem szükséges, sőt nem is kívánatos, hogy azon gondolkozzon, vajon a derékszögű háromszög hogyan számítja ki a saját kerületét és területét, egyáltalán milyen adatokat tárol (például a derékszögű háromszög tárolja-e az átfogó hosszát vagy sem). Előfordulhat, hogy az osztály adattagjai összefüggenek egymással (ha változik az egyik befogó hossza, nyilván változik az átfogó hossza is), ekkor az osztály "felelőssége", hogy valamelyik adattagjának változása esetén a többit is megfelelően változtassa. Ez viszont csak úgy lehetséges, ha nem engdejük, hogy az osztály adattagait az osztályon kívülről módosítsák. Ezért definiáljuk ezeket private láthatóságúnak. 

Ahhoz, hogy mégis lehessen módosítani az osztály adattagait, tipikusan getter és setter függvényeket (getA, getB; setA, setB) veszük fel:

DerekszoguHaromszog.java:

```
public class DerekszoguHaromszog {
    private double a;
    private double b;

    public DerekszoguHaromszog(double a, double b) {
        this.a=a;
        this.b=b;
    }

    public double getA() {
        return a;
    }

    public void setA(double a) {
        this.a = a;
    }

    public double getB() {
        return b;
    }

    public void setB(double b) {
        this.b = b;
    }

    public double kerulet() {
        double c = Math.sqrt( Math.pow(a,2) + Math.pow(b,2) );
        return a+b+c;
    }

    public double terulet() {
        return a*b/2;
    }
}
```

Teglalap.java:

```
public class Teglalap {
    private double a;
    private double b;

    public Teglalap(double a, double b) {
        this.a = a;
        this.b = b;
    }

    public double getA() {
        return a;
    }

    public void setA(double a) {
        this.a = a;
    }

    public double getB() {
        return b;
    }

    public void setB(double b) {
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

Ezt követően akár meg is változtathatjuk a derékszögű háromszögeket reprezentáló osztály "belső működését" anélkül, hogy a program többi részén bármit is változtatni kellene. Példaként tételezzük fel, hogy a derékszögű háromszög mostantól előre kiszámolja az átfogó hosszát.

DerekszoguHaromszog.java:

```
public class DerekszoguHaromszog {
    private double a;
    private double b;
    private double c;

    public DerekszoguHaromszog(double a, double b) {
        this.a=a;
        this.b=b;
        calculate_c();
    }

    private void calculate_c() {
        c =  Math.sqrt( Math.pow(a,2) + Math.pow(b,2) );
    }
    public double getA() {
        return a;
    }

    public void setA(double a) {
        this.a = a;
        calculate_c();
    }

    public double getB() {
        return b;
    }

    public void setB(double b) {
        this.b = b;
        calculate_c();
    }

    public double kerulet() {
        return a+b+c;
    }

    public double terulet() {
        return a*b/2;
    }
}
```

## 4. lépés: interfészek

A derkészögű háromszög és a téglalap is síkidom. Szükségünk lehet olyan metódusokra, amelyek tetszőleges síkidomok esetében használhatóak. Példaként tételezzük fel, hogy van egy metódusunk, amely kiszámolja egy síkidom területének ötszörösét és azt szeretnénk, hogy ez ne csak téglalappal (vagy csak derékszögű háromszöggel) működjön, hanem minden olyan síkidommal, amely ki tudja számolni a saját területét. 

A metódus tehát így néz ki:

```
    public static double terulet_otszorose(Sikidom s) {
        return 5 * s.terulet();
    }
```

Ha ezt a metódust most létrehozzuk a Main.java-ban, hibaüzenetet kapunk. Hogyan írhajtuk le azt, hogy a **Sikidom** egy olyan valami, ami rendelkezik egy olyan függvénnyel, amellyel lehetővé teszi a területének lekérdezését? Ehhez az alábbi interfészt fogjuk definiálni, amely tehát azt mondja meg, hogy a **Sikidom** rendelkezik egy területlekérdezést, valamint egy kerületlekérdezést lehetővé tevő függvénnyel:

Sikidom.java:

```
public interface Sikidom {
    public double kerulet();
    public double terulet();
}
```

Ezt követően a hibaüzenet eltűnik az előbbi függvény definíciójánál, ugyanakkor ezt a függvényt még nem tudjuk meghívni egy téglalappal vagy egy derékszögű háromszöggel, mert nem mondtuk meg, hogy ezek síkidomok. Ehhez módosítjuk a két osztály definícióját:

Teglalap.java:

```
public class Teglalap implements Sikidom {
   ...
}
```

DerekszoguHaromszog.java:

```
public class DerekszoguHaromszog implements Sikidom {
   ...
}
```

Az előbbi metódust mostmár meghívhatjuk akár egy téglalappal, akár egy derékszögű háromszöggel:

Main.java:

```
public class Main {

    public static double terulet_otszorose(Sikidom s) {
        return 5 * s.terulet();
    }

    public static void main(String[] args) {
        // Egy a=10 és b=20 oldalhosszusagu teglalap kerulete es terulete
        Teglalap tgl = new Teglalap(10, 20);
        double k = tgl.kerulet();
        double t = tgl.terulet();

        // Egy a_h=3 es b_h=4 befogokkal rendelkezo derekszogu haromszog
        // kerulete es terulete
        DerekszoguHaromszog h = new DerekszoguHaromszog(3,4);
        double k_h = h.kerulet();
        double t_h = h.terulet();

        System.out.println("A teglalap kerulete: "+k+", terulete: "+t);
        System.out.println("A derekszogu haromszog kerulete: "+k_h+", terulete: "+t_h);

        System.out.println(terulet_otszorose(tgl));
        System.out.println(terulet_otszorose(h));
    }
}
```

Egy Sikidom "típusú" változóban pedig akár téglalapot, akár derékszögű hároszöget tárolhatunk:

Main.java:

```
public class Main {
    ...   
    public static void main(String[] args) {
        ...
        Teglalap tgl = new Teglalap(10, 20);
        ...
        Sikidom si = tgl;
    }
}
```

## 5. lépés: leszármaztatott osztályok, öröklés

A négyzet egy olyan speciális téglalap, amelynek szélessége megegyezik a magasságával. Éppenséggel egy teljesen új osztályt is létrehozhatnánk a négyzeteknek, de inkább kihasználjuk az előbbi megfigyelést, és "leszármaztatjuk" a négyzet osztályt a téglalap osztályból (kódban: "extends Teglalap"). A leszármaztatott "Negyzet" osztály örökli a "Teglalap" tulajdonságait, metódusait (azaz például akkor is van kerület- és területszámító metódusa, ha ezt nem implementáltuk kölün) és ráadásul, mivel a téglalap használható "Sikidom"-ként, ezért a "Negyzet" is használható lesz "Sikidom"-ként is. Arra viszont figyelnünk kell, hogy ha valaki megváltoztatná a "Negyzet" akármelyik oldalhosszúságát is, akkor - a téglalappal ellentétben - a másik oldal hossza is változik, ezért felülírjuk a "Teglalap" setA() és setB() metódusait. Az ősosztály, azaz a "Teglalap" metódusaira a "super" referencián keresztül hivatkozhatunk: a példában az új setA() és setB() metódus meghívja az ősosztály setA() és setB() metódusait:

Negyzet.java:

```
public class Negyzet extends Teglalap {
    public Negyzet(double a) {
        super(a,a); // A Teglalap konstruktorát hívjuk
    }
    public void setA(double a) {
        super.setA(a);
        super.setB(a);
    }
    public void setB(double b) {
        super.setA(b);
        super.setB(b);
    }
}
```

Main.java:

```
public class Main {

    public static double terulet_otszorose(Sikidom s) {
        return 5 * s.terulet();
    }

    public static void main(String[] args) {
        ...
        Sikidom si = new Negyzet(12);
        System.out.println(terulet_otszorose(si));
    }
}
```

## 6. lépés: osztályszintű (statikus) metódusok

Egyes metódusok nem az objektumpéldányhoz, hanem az osztály egészéhez kapcsolódnak, és ezért az osztály példányosítása nélkül is meghívhatóak. Példa:

Negyzet.java:

```
public class Negyzet extends Teglalap {
    ...    
    public static String getVersion() {
        return "1.0";
    }
}
```

Main.java:

```
public class Main {
    ...
    public static void main(String[] args) {
        ...
        System.out.println(Negyzet.getVersion());
    }
}
```

A "régi ismerősünk", a *public static void main(String[] args)* is egy ilyen statikus metódus.

## 7. lépés: tömblisták (ArrayList)

Egy tömblistában tömbszerűen tárolhatunk objektumpéldányokat. Lásd: [https://www.w3schools.com/java/java_arraylist.asp](https://www.w3schools.com/java/java_arraylist.asp) .

A tömblista elemeit (ha pl. számokat vagy String-eket tárolunk benne) a Collection.sort() metódussal tudjuk rendezni.

## 8. lépés: absztrakt osztályok


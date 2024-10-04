# Objektumorientált programozás 

Az objektumorientált programozás legfontosabb fogalmait egy példán keresztül mutatjuk be lépésről lépésre.
Konkrétan síkidomok kerületét és területét fogjuk számolni Java-ban írt kód segítségével.

## 0. Lépés 

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

## 1. Lépés 

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

## 2. lépés

Az előbbi megoldás mellett lehetséges, hogy a háromszög kerület- és területszámító függvényeit használjuk
a téglalap adataira, vagy éppen fordítva. Az objektum-orientált programozás alapgondolata, hogy az 
adatokat és a hozzájuk kapcsolódó függvényeket egy egységbe, ún. osztályba, szervezzük. Ezért defináljuk 
a Teglalap és DerekszoguHaromszog osztályokat a Teglalap.java illetve DerekszoguHaromszog.java elnevezésű
fájlokban. A főprogramban ezeket az osztályokat fogjuk példányosítani, más szóval: létrehozzuk a konkrét
derékszögű háromszögnek és a konkrét téglalapnak megfelelő objektumokat a **new** kulcsszó használatával. 
Ekkor adjuk meg azon téglalap és háromszög adatait (oldalhosszakat), amelyekkel ténylegesen dolgozunk a programban.

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
    }

    public double kerulet() {
        return 2*a+2*b;
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

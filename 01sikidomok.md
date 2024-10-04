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
FOLYT. KÖV.

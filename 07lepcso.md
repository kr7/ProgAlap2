# Hányféleképpen lehet felmenni egy $n$ lépcsőfokból álló lépcsőn...

...ha minden lépésben egy vagy két lépcsőfokot tudunk lépni? Ez egy tipikus feladat állásinterjúkon programozáshoz kapcsolódó poziciók esetén.

Egy 1 lépcsőfokból álló lépcsőn nyilván 1-féleképpen tudunk felmenni (lépünk egyet és kész).

Egy 2 képcsófokból álló lépcső esetében vagy egyet lépünk és ekkor a következő lépésben kell még egyet lépnünk; vagy rögtön két lépcsőfokot lépünk és akkor rögtön fent vagyunk a lépcső tetején. Ez tehát két lehetőség.

Könnyen látható, hogy egy három lépcsőfokból álló lépcsőn 3-féleképp tudunk felmenni (1+1+1, 1+2, 2+1). 

De mi a helyzet egy $n$ lépcsőfokból álló lépcsővel? Az első lépésben vagy egyet lépünk felfelé vagy kettőt, és attól függően, hogy egyet vagy kettőt léptünk, a továbbiakban vagy egy $n-1$ vagy $n-2$ lépcsőfokból álló lépcsőn kell felmennünk. 

Eddigi megfontolásaink alapján közvetlenül adódik az alábbi rekurzív függvény (Javaban: statikus metódus), amely kiszámolja a kérdésre a választ:

```
  public static long f_rekurziv(long n) {
        if (n==1) {
            return 1;
        }
        if (n==2) {
            return 2;
        }
        return f_rekurziv(n-1)+f_rekurziv(n-2);
    }
```

Ez azonban, a gép teljesítményétől függően, már viszonylag kis $n$-ek (pl. $n=60$) esetén is nagyon sokáig fog futni. Gondoljuk végig, hogy mi a gond ezzel a függvénnyel! Az, hogy ugyanazt a részeredményt nagyon sokszor számolja ki, 
a futásideje (komplexitása) exponenciális $n$-ben nézve.

Nézzünk egy olyan változatot, amely egy tömböt hoz létre és a tömb $i$-dek eleme azt tárolja, hogy egy $i+1$ lépcsőfokú lépcsőn hányféleképp lehet felmenni. (Igazából a tömb $i$-dik eleme azt is tárolhatná, hogy egy $i$ hosszú lépcsőn 
hányféleképp lehet felmenni, de Javaban a tömb indexálása 0-val kezdődik, nem tudunk olyan tömböt létrehozni, aminek nincs 0-dik eleme, viszont nincs szükségünk arra, hogy a 0 hosszúságú lépcsőn hányféleképp lehet felmenni, hiszen
ennek a kérdésnek nincs sok értelme. Tehát amiatt, hogy a tömb $i$-dek eleme azt tárolja, hogy egy $i+1$ lépcsőfokú lépcsőn hányféleképp lehet felmenni, egyel rövidebb tömbre van szükségünk.)

```
    public static long f_tombbel(int n) {
        long[] lepcso = new long[n];
        // 0-dik elem: 1 hosszusagu lepcson hanyfelekeppen tudok felmenni
        // 1-dik elem: 2 hosszusagu lepcson hanyfelekeppen tudok felmenni
        // i-dik elem: i+1 hosszusagu lepcson hanyfelekeppen tudok felmenni

        lepcso[0] = 1;
        lepcso[1] = 2;

        for (int i=2;i<n;i++) {
            lepcso[i] = lepcso[i-1]+lepcso[i-2];
        }
        return lepcso[n-1];
    }
```

Így persze még mindig relatíve sok memóriát használunk ahhoz képest, hogy a for-ciklusban végzett számítás során mindig csak a tömb utolsó két elemére van szükség. Egy 1000 lépcsőfokból álló lépcső esetében például 1000 számnak foglalunk 
helyet, mindig csak a tömb utolsó két elemére van szükségünk. Egy kevesebb memóriát használó megoldás az alábbi:

```
  public static long f_ketvaltozos(int n) {
        long a = 1;
        long b = 2;

        for (int i=2;i<n;i++) {
            long c = a+b;

            a = b;
            b = c;
        }
        return b;
    }
```

Vajon tudunk-e olyan megoldást adni, ami még ennél is lényegesen hatékonyabb? Aki tanult elméleti számítástudományt, bizonyára időközben észrevette, hogy valójában egy Fibonacci-sorozat elmeit számolgatjuk. Pontosabban:
az $n$ hosszúságú lépcsőn annyiszor tudunk felmenni, amennyi a Fibonacci-sorozat $n+1$-dik eleme. A Fibonacci-sorozat $n$-dik eleme viszont megadható zárt alakban (azaz létezik egy képlet, ami közvetlenül ezt számolja, ld. pl. 
[https://de.wikipedia.org/wiki/Fibonacci-Folge](https://de.wikipedia.org/wiki/Fibonacci-Folge) ), ezért el tudjuk
kerülni a for-ciklust: 

```
import Math.*;

...

    public static long f(int n) {
        double gyok5 = sqrt(5);
        return (long) ((pow((1+gyok5)/2,n+1) - pow((1-gyok5)/2,n+1))/gyok5);
    }
```

Végezetül: vajon érdemes-e a $\sqrt{5}$ számítását kiemelni, ahogy ez előbbi kódban látható? Egyrészt persze érvelhetünk azzal, hogy a gyökvonás processzorszintű utasítások darabszámában számolva egy viszonylag "költséges" művelet, 
ezért jogos, hogy csak egyszer akarjuk elvégezni, másrészt viszont az ilyen optimalizálásokat a modern fordítóprogramok jellemzően automatikusan elvégzik.


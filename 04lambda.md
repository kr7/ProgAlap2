# Funcionális interface-k, lambda kifejezések

A [funkcionális interface](https://www.javatpoint.com/java-8-functional-interfaces)-k olyan interface-k, amelyek egyetlen implementálandó metódust tartalmaznak.
Amint látni fogjuk, funkcionális interface-ket megvalósíthatunk ún. lambda kifejezésekkel, ami lényegében nem más, mint egy metódus tömör leírása.


## 0. lépés: tömblista

Létrehozunk egy tömblistát. A későbbiekben ezen tömblista minden elemére alkalmazni fogjuk ugyanazt a műveletet.

Main.java:

```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<Integer>();

        numbers.add(23);
        numbers.add(12);
        numbers.add(7);
        numbers.add(9);
        numbers.add(95);
}
```

## 1. lépés: forEach() metódus

A forEach() metódus lehetővé teszi, hogy ugyanazt a műveletet a tömblista minden egyes elemén elvégezzük. A forEach() egy olyan objektumot vár bemenetül, amely 
implementálja a *java.util.function.Consumer* interface-t. 

Eddigi ismereteink alapján tehát implementálhatunk egy osztályt, amely megvalósítja a *java.util.function.Consumer* interface-t. Korábban minden 
osztályt az osztály elnevezésével azonos nevű fájlban hoztunk létre, de valójában akár egy metóduson belül is létrehozhatunk osztályokat, viszont ezek 
csak az adott metóduson belül lesznek használhatók.

Main.java:

```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<Integer>();

        numbers.add(23);
        numbers.add(12);
        numbers.add(7);
        numbers.add(9);
        numbers.add(95);

        class EgyHozzaadasa implements java.util.function.Consumer {
            @Override
            public void accept(Object o) {
                System.out.println( ((int) o) + 1);
            }
        }

        numbers.forEach(new EgyHozzaadasa());
}
```

## 2. lépés: anoním osztály

Mivel az *EgyHozzadasa* osztályt csak egyetlenegyszer használjuk (amikor példányosítjuk), ezért helyette egy anoním osztályt is
példányosíthatunk a forEach() metódus paramétereként.

Main.java:

```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<Integer>();

        numbers.add(23);
        numbers.add(12);
        numbers.add(7);
        numbers.add(9);
        numbers.add(95);

        numbers.forEach( new java.util.function.Consumer() {
            public void accept(Object o) {
                System.out.println( ((int) o) + 1);
            }
        } );
}
```

## 3. lépés: lambda kifejezés

Mivel az előbbi anoním osztályunk egyetlen metódust tartalmaz csak, ezért használhatunk helyette egy lambda kifejezést is.

Main.java:

```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<Integer>();

        numbers.add(23);
        numbers.add(12);
        numbers.add(7);
        numbers.add(9);
        numbers.add(95);

        numbers.forEach( (o) -> { System.out.println( ((int)o) + 1 ); } );
}
```

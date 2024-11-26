# Leképezések (Map)

A leképezés (java.util.Map) olyan adatstruktúra, amelyben kulcs-érték párokat tárolhatunk. A Set interface-hez hasonlóan a Map-nek is több implementációja létezik, úgy mint HashMap és TreeMap.
A Map használatát az alábbi példa illusztrálja:

Main.java:

```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        HashMap<Integer, String> elso_hashmapem = new HashMap<Integer, String>();
        elso_hashmapem.put( 1, "Esztergom");
        elso_hashmapem.put( 2, "Budapest");
        elso_hashmapem.put( 3, "Szeged");
        elso_hashmapem.put( 4, "Pecs");

        System.out.println(elso_hashmapem);
        System.out.println(elso_hashmapem.get(3));
        elso_hashmapem.clear(); // Mindent kitörlünk a mapből
        System.out.println(elso_hashmapem);
        System.out.println(elso_hashmapem.get(3));
    }
}
```

További olvasnivaló: [https://www.w3schools.com/java/java_hashmap.asp](https://www.w3schools.com/java/java_hashmap.asp)

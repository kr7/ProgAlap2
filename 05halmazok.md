## Halmazok (Set)

Az alábbi kód a halmaz (java.util.Set) interface-t megvalósító HashSet néhány metódusának működését szemlélteti:

Main.java:

```
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<String> cars = new HashSet<String>();
        cars.add("Volvo");
        cars.add("BMW");
        cars.add("Ford");
        cars.add("BMW"); // hiába tesszük be kétszer, csak egyszer lesz bent a halmazban! 
        cars.add("Mazda");
        cars.add("Suzuki");
        System.out.println(cars);

        System.out.println(cars.contains("Suzuki"));

        cars.remove("BMW"); 
        // hiába tettem be két BMW-t, mivel csak egyszer van benne a halmazban, 
        // ezért ha egyszer(!) kiveszem belőle, akkor utána már nincs benne
        System.out.println(cars);

        cars.clear();
        System.out.println(cars);   
    }
}
```

A HashSet helyett az előbbi példában használhatunk TreeSet-et is: mindkét osztály implementálja a Set interface-t, ezért mindkettő rendelkezik a Set interface-ben
megadott metódusokkal. A két osztály között a különbség az, hogy máshogyan tárolják az adatokat (és ezért lehet, hogy az egyik projektben az egyik, a másikban a másik
működése lesz gyorsabb). 

További olvasnivaló: [https://www.w3schools.com/java/java_hashset.asp](https://www.w3schools.com/java/java_hashset.asp)

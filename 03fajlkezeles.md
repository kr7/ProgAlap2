# Fájlkezelés

## 1. Néhány alapvető fájlművelet

Tekintsük a [https://www.w3schools.com/java/java_files.asp](https://www.w3schools.com/java/java_files.asp) oldalon található leírást, és próbáljunk ki néhányat a File metódusai közül:

```
import java.io.*;

public class Main {
    public static void main(String[] args) {
        File f = new File("src/proba.txt");

        // Fájl létezésének ellenőrzése        
        System.out.println(f.exists());

        // Fájl teljes elérési útja a fájlrendszerben
        System.out.println(f.getAbsolutePath());

        // A fájl mérete (hossza)
        System.out.println(f.length());

        // Vegyük észre, hogy a mappák (könyvtárak) is fájlként használhatóak
        File f1 = new File("C:\\Users\\buzaka\\IdeaProjects\\ProgAlapjai2.2\\src");
        System.out.println(f1.exists());

        // Mappabeli fájlok listázása
        String[] mappa_tartalma = f1.list();
        for (String s : mappa_tartalma) {
            System.out.println(s);
        }
    }
}
```

## 2. Szövegfájlok

Annak ellenére, hogy a mai számítógépek használata közben már nem nyilvávaló, valójában minden adatot 0-k és 1-k sorozataként tárolunk. Ez egyaránt igaz a fájlban történő tárolás esetén valamilyen háttértáron (merevlemez, SSD, stb.), 
de akkor is ha az adat a memóriában van. 

Az, hogy a 0-k és 1-k sorozatát egy (kettes számrendszerbeli) számnak tekintjük-e, vagy valamilyen betűnek (esetleg egy más jelnek, matematikai jelnek, görög betűnek, kínai vagy japán írásjelnek, stb.) feleltetjük meg, csak rajtunk múlik. 

Példaként tekintsük az alábbi sorozatokat kettes számrendszerbeli számként:

```
01000001 -> 65
01000010 -> 66
01000011 -> 67
...
```

Ugyanezen sorozatok megfeleltethetők az ábécé beűinek is:

```
01000001 -> A
01000010 -> B
01000011 -> C
... 
```

A fájlok olvasásakor és írásakor lényeges, hogy milyen megfeleltetést használunk. Mi a következőkben szövegfájlokkal fogunk dolgozni, azaz a 0-k és 1-k sorozatát betűknek fogjuk megfeleltetni. Amint látni fogjuk, a hozzárendelést
nem kell megadnunk, elég, ha a szövegfájlok használatára készített osztályokat (illetve azok példányait) használjuk. 

Szövegfájlok írására az alábbiakban mutatunk egy példát, eközben az esetlegesen adódó hibákat (IOException) elkapjuk és kiírjuk a standard outputra. **FIGYELEM: ha a fájl már létezik, akkor ezzel felülírjuk!** 

Main.java:

```
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try {
            FileWriter myWriter = new FileWriter("elsofajlom.txt");
            myWriter.write("A fajlkezeles Javaban elsore bonyolultnak tunhet, " +
                           "de aki megtanulja, annak gyerekjatek!");
            myWriter.close();
            System.out.println("Sikeresen kiirtuk az adatot.");
        } catch (IOException e) {
            System.out.println("Hiba tortent!");
            e.printStackTrace();
        }
    }
}
```


Falj olvasasa is hasonlóképpen lehetséges:

```
...

try {
    File f = new File("elsofajlom.txt");
    Scanner reader = new Scanner(f);
    while (reader.hasNextLine()) {
        String sor = reader.nextLine();
            System.out.println(sor);
        }
        reader.close();
    } catch (FileNotFoundException e) {
        System.out.println("Hiba történt.");
        e.printStackTrace();
    }

...
```

Manapság a gájlok írására és olvasására inkább try-with-resources blokkot használunk. 
Ekkor nem szükséges a fájlokat lezárnunk, mert ez automatikusan megtörténik:

Main.java:

```
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try (FileWriter fw = new FileWriter("src/proba.txt");)
        {
            fw.write("Fajlba iras a try-with-resources blokkal.");
        } catch (IOException e) {
            System.out.println(e);
        }

        File f = new File("src/proba.txt");
        try (Scanner reader = new Scanner(f); ) {
            while (reader.hasNextLine()) {
                String sor = reader.nextLine();
                System.out.println(sor);
            }
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```


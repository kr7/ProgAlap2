# MÁSODIK ZH MINTA

Adott a köröket reprezentáló Kor osztály alábbi implementációja:

Kor.java:

```
public class Kor {
	private double x; // a kor középpontjának vizszintes koordinataja
	private double y; // a kor középpontjának fuggoleges koordinataja
	private double r; // a kor sugara

	public Kor(double x, double y, double r) {
		this.x = x; this.y = y; this.r = r;
	}
}
```

(a) [10 pont] Hozzon létre egy saját kivételt (exception-t) arra az esetre, ha a kör sugara negatív!
(b) [10 pont] A Kor osztaly konstruktorát módosítsa olyan módon, hogy az előző pontban létrehozott kivételt dobja abban az esetben, ha r negatív!
(c) [40 pont] Hozzon létre egy "főprogramot", amelyben példányosítja a Kor osztályt! A Kor adatait egy szövegfájlból olvassa be! 
Feltételezheti, hogy a Kor adatait a fájl első három sora tartalmazza. Ne feledkezzem meg a kivételek kezeléséről: kapja el a lehetséges kivételeket és írjon ki beszédes hibaüzeneteket!

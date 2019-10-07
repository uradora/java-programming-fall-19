---
path: '/osa-6/2-separating-user-interface-from-program-logic'
title: 'Separating the user interface from program logic'
hidden: true
---

<text-box variant='learningObjectives' name='Learning objectives'>

<!-- - Tutustut sovelluksen luomiseen siten, että käyttöliittymä ja sovelluslogiikka ovat erillään. -->
<!-- - Osaat luoda tekstikäyttöliittymän, joka saa parametrinaan sovelluskohtaisen sovelluslogiikan sekä syötteen lukemiseen käytettävän Scanner-olion. -->
 - Understand creating programs so that the user interface and the application logic are separated
 - Can create a textual user interface which takes program specific application logic and a Scanner object as parameters


</text-box>

<!-- Tarkastellaan erään ohjelman rakennusprosessia sekä tutustutaan sovelluksen vastuualueiden erottamiseen toisistaan. Ohjelma kysyy käyttäjältä sanoja kunnes käyttäjä syöttää saman sanan uudestaan. Ohjelma käyttää listaa sanojen tallentamiseen. -->
Let's examine the process of implementing a program and separating different areas of responsibility from each other.
The program asks user to write words until the user writes the same word twice.

<sample-output>

Write a word: **carrot**
Write a word: **turnip**
Write a word: **potato**
Write a word: **celery**
Write a word: **potato**
You gave the same word twice!

</sample-output>

<!-- Rakennetaan ohjelma osissa. Eräs haasteista on se, että on vaikea päättää miten lähestyä tehtävää, eli miten ongelma tulisi jäsentää osaongelmiksi, ja mistä osaongelmasta kannattaisi aloittaa. Yhtä oikeaa vastausta ei ole -- joskus on hyvä lähteä pohtimaan ongelmaan liittyviä käsitteitä ja niiden yhteyksiä, joskus taas ohjelman tarjoamaa käyttöliittymää. -->
Let's build this program piece by piece. One of the challenges is, that it is difficult to decide how to approach the problem, or how to split the problem into smaller subproblems, and from which subproblem to start.
There is no one clear answer -- sometimes it is good to start from the problem domain and its concepts and their connections, sometimes it is better to start from the user interface.

<!-- Käyttöliittymän hahmottelu voisi lähteä liikenteeseen luokasta Kayttoliittyma. Käyttöliittymä käyttää syötteen lukemiseen Scanner-oliota, joka annetaan sille käyttöliittymän luonnin yhteydessä. Tämän lisäksi käyttöliittymällä on käynnistämiseen tarkoitettu metodi. -->
We could start implementing the user interface by creating a class UserInterface. The user interface uses a Scanner object for reading user input. This object is given to the User Interface as the interface is created. In addition to this, the user interface has a separate method for starting up the interface.


<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
    }

    public void kaynnista() {
        // tehdään jotain
    }
}
``` -->
```java
public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {
        // do something
    }
}
```

<!-- Käyttöliittymän luominen ja käynnistäminen onnistuu seuraavasti.
 -->
 Creating and starting up a user interface can be done as follows.

<!-- ```java
public static void main(String[] args) {
    Scanner lukija = new Scanner(System.in);
    Kayttoliittyma kayttoliittyma = new Kayttoliittyma(lukija);
    kayttoliittyma.kaynnista();
}
``` -->
```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    UserInterface userinterface = new userinterface(scanner);
    userinterface.start();
}
```

<!-- ## Toisto ja lopetus

Ohjelmassa on (ainakin) kaksi "aliongelmaa". Ensimmäinen on sanojen toistuva lukeminen käyttäjältä kunnes tietty ehto toteutuu. Tämä voitaisiin hahmotella seuraavaan tapaan.
 -->
## Looping and quitting

This program has (at least) two "sub-problems". The first problem is continuously reading words from the user until a certain condition is reached. We can outline this as follows.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (*pitää lopettaa*) {
                break;
            }

        }

        System.out.println("Annoit saman sanan uudestaan!");
    }
}
``` -->
```java
public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (*stop condition*) {
                break;
            }

        }

        System.out.println("You gave the same word twice!");
    }
}
```

<!-- Sanojen kysely jatkuu kunnes käyttäjä syöttää jo aiemmin syötetyn sanan. Täydennetään ohjelmaa siten, että se tarkastaa onko sana jo syötetty. Vielä ei tiedetä miten toiminnallisuus kannattaisi tehdä, joten tehdään siitä vasta runko.
 -->
The program continues to ask for words until the user enters a word that has already been entered before. Let us modify the program so that it checks whether the word has been entered or not. We don't know yet how to implement this functionality, so let us first build an outline for it.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (onJoSyotetty(sana)) {
                break;
            }

        }

        System.out.println("Annoit saman sanan uudestaan!");
    }

    public boolean onJoSyotetty(String sana) {
        // tänne jotain

        return false;
    }
}
``` -->
```java
public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (alreadyEntered(word)) {
                break;
            }

        }

        System.out.println("You gave the same word twice!");
    }

    public boolean alreadyEntered(String word) {
        // do something here

        return false;
    }
}
```

<!-- Ohjelmaa on hyvä testata koko ajan, joten tehdään metodista kokeiluversio:
 -->
It's a good idea to test the program continuously, so let's make a test version of the method:

<!-- ```java
public boolean onJoSyotetty(String sana) {
    if (sana.equals("loppu")) {
        return true;
    }

    return false;
}
``` -->
```java
public boolean alreadyEntered(String word) {
    if (word.equals("end")) {
        return true;
    }

    return false;
}
```

<!-- Nyt toisto jatkuu niin kauan kunnes syötteenä on sana loppu:
 -->
Now the loop continues until the input equals the word "end":

<!-- <sample-output>

Anna sana: **porkkana**
Anna sana: **selleri**
Anna sana: **nauris**
Anna sana: **lanttu**
Anna sana: **loppu**
Annoit saman sanan uudestaan!

</sample-output> -->
<sample-output>

Enter a word: **carrot**
Enter a word: **celery**
Enter a word: **turnip**
Enter a word: **end**
You gave the same word twice!

</sample-output>

<!-- Ohjelma ei toimi vielä kokonaisuudessaan, mutta ensimmäinen osaongelma eli ohjelman pysäyttäminen kunnes tietty ehto toteutuu on saatu toimimaan.
 -->
The program doesn't completely work yet, but the first sub-problem - quitting the loop when a certain condition has been reached - has now been implemented.

<!-- ## Oleellisten tietojen tallentaminen

Toinen osaongelma on aiemmin syötettyjen sanojen muistaminen. Lista sopii mainiosti tähän tarkoitukseen.
 -->

## Storing relevant information

Another sub-problem is remembering the words that have already been entered. A list is a good structure for this purpose.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;
    private ArrayList<String> sanat;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.sanat = new ArrayList<String>();
    }

    //...
}
``` -->
```java
public class UserInterface {
    private Scanner scanner;
    private ArrayList<String> words;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.words = new ArrayList<String>();
    }

    //...
}
```

<!-- Kun uusi sana syötetään, on se lisättävä syötettyjen sanojen joukkoon. Tämä tapahtuu lisäämällä while-silmukkaan listan sisältöä päivittävä rivi:
 -->
When a new word is entered, it has to be added into the list of words that have been entered before. This is done by adding a line that updates our list to the while-loop:

<!--
```java
while (true) {
    System.out.print("Anna sana: ");
    String sana = lukija.nextLine();

    if (onJoSyotetty(sana)) {
        break;
    }

    // lisätään uusi sana aiempien sanojen listaan
    this.sanat.add(sana);
}
``` -->
```java
while (true) {
    System.out.print("Enter a word: ");
    String word = scanner.nextLine();

    if (alreadyEntered(word)) {
        break;
    }

    // adding the word to the list of previous words
    this.words.add(word);
}
```

<!-- Kayttoliittyma näyttää kokonaisuudessaan seuraavalta.
 -->
The whole user interface looks as follows.

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;
    private ArrayList<String> sanat;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.sanat = new ArrayList<String>();
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (onJoSyotetty(sana)) {
                break;
            }

            // lisätään uusi sana aiempien sanojen listaan
            this.sanat.add(sana);
        }

        System.out.println("Annoit saman sanan uudestaan!");
    }

    public boolean onJoSyotetty(String sana) {
        if (sana.equals("loppu")) {
            return true;
        }

        return false;
    }
}
``` -->
```java
public class UserInterface {
    private Scanner scanner;
    private ArrayList<String> words;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.words = new ArrayList<String>();
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (alreadyEntered(word)) {
                break;
            }

            // adding the word to the list of previous words
            this.words.add(word);

        }

        System.out.println("You gave the same word twice!");
    }

    public boolean alreadyEntered(String word) {
       if (word.equals("end")) {
            return true;
        }

        return false;
    }
}


/* Jälleen kannattaa testata, että ohjelma toimii edelleen. Voi olla hyödyksi esimerkiksi lisätä kaynnista-metodin loppuun testitulostus, joka varmistaa että syötetyt sanat todella menivät listaan. */
Again it is a good idea to test that the program still works. For example, it might be useful to add a test print to the end of the start-method to make sure that the entered words have really been added to the list.

/* ```java
// testitulostus joka varmistaa että kaikki toimii edelleen
for(String sana: this.sanat) {
    System.out.println(sana);
}
``` */
```java
// test print to check that everything still works
for (String word: this.words) {
    System.out.println(word);
}
```

<!-- ## Osaongelmien ratkaisujen yhdistäminen

Muokataan vielä äsken tekemämme metodi `onJoSyotetty` tutkimaan onko kysytty sana jo syötettyjen joukossa, eli listassa.
 -->
## Combining the solutions to sub-problems

Let's change the method 'alreadyEntered' so that it checks whether the entered word is contained in our list of words that have been already entered.


<!-- ```java
public boolean onJoSyotetty(String sana) {
    return this.sanat.contains(sana);
}
```
 -->
```java
public boolean alreadyEntered(String word) {
    return this.words.contains(word);
}
```
<!-- Nyt sovellus toimii kutakuinkin halutusti.
 -->
Now the application works as intended.

<!-- ## Oliot luonnollisena osana ongelmanratkaisua

Rakensimme äsken ratkaisun ongelmaan, missä luetaan käyttäjältä sanoja, kunnes käyttäjä antaa saman sanan uudestaan. Syöte ohjelmalle oli esimerkiksi seuraavanlainen.
 -->
## Objects as a natural part of problem solving

We just built a solution to a problem where the program reads words from a user until the user enters a word that has already been entered before. Our example input was as follows:

<!-- <sample-output>

Anna sana: **porkkana**
Anna sana: **selleri**
Anna sana: **nauris**
Anna sana: **lanttu**
Anna sana: **selleri**
Annoit saman sanan uudestaan!

</sample-output> -->
Enter a word: **carrot**
Enter a word: **celery**
Enter a word: **turnip**
Enter a word: **end**
You gave the same word twice!

<!-- Päädyimme ratkaisuun
 -->
We came up with the following solution:

<!-- ```java
public class Kayttoliittyma {
    private Scanner lukija;
    private ArrayList<String> sanat;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.sanat = new ArrayList<String>();
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (onJoSyotetty(sana)) {
                break;
            }

            // lisätään uusi sana aiempien sanojen listaan
            sanat.add(sana);
        }

        System.out.println("Annoit saman sanan uudestaan!");
    }

    public boolean onJoSyotetty(String sana) {
        return this.sanat.contains(sana);
    }
}
``` -->
```java
public class UserInterface {
    private Scanner scanner;
    private ArrayList<String> words;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.words = new ArrayList<String>();
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (alreadyEntered(word)) {
                break;
            }

            // adding the word to the list of previous words
            this.words.add(word);

        }

        System.out.println("You gave the same word twice!");
    }

    public boolean alreadyEntered(String word) {
       if (word.equals("end")) {
            return true;
        }

        return false;
    }
}


/* Ohjelman käyttämä apumuuttuja lista `sanat` on yksityiskohta käyttöliittymän kannalta. Käyttöliittymän kannaltahan on oleellista, että muistetaan niiden *sanojen joukko* jotka on nähty jo aiemmin. Sanojen joukko on selkeä erillinen "käsite", tai abstraktio. Tälläiset selkeät käsitteet ovat potentiaalisia olioita; kun koodissa huomataan "käsite" voi sen eristämistä erilliseksi luokaksi harkita.*/
From the point of view of the user interface, the support variable 'words' is just a detail. The main thing is that the user interface remembers the *set of words* that have been entered before. The set is a clear distinct "concept" or an abstraction. Distinct concepts like this are all potential objects: when we notice that we have an abstraction like this in our code, we can think about separating the concept into a class of its own.

/* ### Sanajoukko

Tehdään luokka `Sanajoukko`, jonka käyttöönoton jälkeen käyttöliittymän metodi `kaynnista` on seuraavanlainen:*/
### Wordset

 Let's make a class called 'Wordset'. After implenting the class, the user interface's start method looks like this:

/* ```java
while (true) {
    String sana = lukija.nextLine();

    if (sanat.sisaltaa(sana)) {
        break;
    }

    sanajoukko.lisaa(sana);
}

System.out.println("Annoit saman sanan uudestaan!");
```*/
```java
while (true) {
    String word = scanner.nextLine();

    if (words.contains(word)) {
        break;
    }

    wordset.add(word);
}

System.out.println("You gave the same word twice!");
```

<!-- Käyttöliittymän kannalta Sanajoukolla kannattaisi siis olla metodit `boolean sisaltaa(String sana)` jolla tarkastetaan sisältyykö annettu sana jo sanajoukkoon ja `void lisaa(String sana)` jolla annettu sana lisätään joukkoon.

Huomaamme, että näin kirjoitettuna käyttöliittymän luettavuus on huomattavasti parempi.

Luokan `Sanajoukko` runko näyttää seuraavanlaiselta:
 -->
From the point of view of the user interface, the class Wordset should contain the method 'boolean contains(String word)', that checks whether the given word is contained in our set of words, and the method 'void add(String word)', that adds the given word into the set.

We notice that the readability of the user interface is greatly improved when it's written like this.

The outline for the class 'Wordset' looks like this:

<!-- ```java
public class Sanajoukko {
    // oliomuuttuja(t)

    public Sanajoukko() {
        // konstruktori
    }

    public boolean sisaltaa(String sana) {
        // sisältää-metodin toteutus
        return false;
    }

    public void lisaa(String sana) {
        // lisaa-metodin toteutus
    }
}
``` -->
```java
public class Wordset {
    // object variable(s)

    public Wordset() {
        // constructor
    }

    public boolean contains(String word) {
        // contains-method implementation
        return false;
    }

    public void add(String word) {
        // add-method implementation
    }
}
```

<!-- ### Toteutus aiemmasta ratkaisusta

Voimme toteuttaa sanajoukon siirtämällä aiemman ratkaisumme listan sanajoukon oliomuuttujaksi:
 -->

### Implementation from an earlier solution

We can implement the set of words by making our earlier solution, the list, into an object variable:

<!-- ```java
import java.util.ArrayList;

public class Sanajoukko {
    private ArrayList<String> sanat;

    public Sanajoukko() {
        this.sanat = new ArrayList<>();
    }

    public void lisaa(String sana) {
        this.sanat.add(sana);
    }

    public boolean sisaltaa(String sana) {
        return this.sanat.contains(sana);
    }
}
``` -->
```java
import java.util.ArrayList;

public class Wordset {
    private ArrayList<String> words

    public Wordset() {
        this.words = new ArrayList<>();
    }

    public void add(String word) {
        this.words.add(word);
    }

    public boolean contains(String word) {
        return this.words.contains(word);
    }
}


/* Ratkaisu on nyt melko elegantti. Erillinen käsite on saatu erotettua ja käyttöliittymä näyttää siistiltä. Kaikki "likaiset yksityiskohdat" on saatu siivottua eli kapseloitua olion sisälle.

Muokataan käyttöliittymää niin, että se käyttää Sanajoukkoa. Sanajoukko annetaan käyttöliittymälle samalla tavalla parametrina kuin Scanner.*/
Now our solution is quite elegant. We have separated a distinct concept into a class and our user interface looks clean. All "dirty details" have been encapsulated neatly inside an object.

Let's now edit the user interface so that it uses the class Words. The class is given to the user interface as a parameter, just like Scanner.

/* ```java
public class Kayttoliittyma {
    private Sanajoukko sanajoukko;
    private Scanner lukija;

    public Kayttoliittyma(Sanajoukko sanajoukko, Scanner lukija) {
        this.sanajoukko = sanajoukko;
        this.lukija = lukija;
    }

    public void kaynnista() {

        while (true) {
            System.out.print("Anna sana: ");
            String sana = lukija.nextLine();

            if (this.sanajoukko.sisaltaa(sana)) {
                break;
            }

            this.sanajoukko.lisaa(sana);
        }

        System.out.println("Annoit saman sanan uudestaan!");
    }
}
``` */
```java
public class UserInterface {
    private Wordset wordset;
    private Scanner scanner;

    public userInterface(Wordset wordset, Scanner scanner) {
        this.wordset = wordset;
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (this.wordset.contains(word)) {
                break;
            }

            this.wordset.add(word);
        }

        System.out.println("You gave the same word twice!");
    }
}
```

<!-- Ohjelman käynnistäminen tapahtuu nyt seuraavasti:
 -->
Starting the program is now done as follows:

<!-- ```java
public static void main(String[] args) {
    Scanner lukija = new Scanner(System.in);
    Sanajoukko joukko = new Sanajoukko();

    Kayttoliittyma kayttoliittyma = new Kayttoliittyma(joukko, lukija);
    kayttoliittyma.kaynnista();
}
``` -->
```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    Wordset set = new Wordset();

    UserInterface userinterface = new UserInterface(set, scanner);
    userinterface.start();
}
```

<!-- ## Luokan sisäisen toteutuksen muuttaminen

Olemme päätyneet tilanteeseen missä `Sanajoukko` ainoastaan "kapseloi" ArrayList:in. Onko tässä järkeä? Kenties. Voimme nimittäin halutessamme tehdä Sanajoukolle muitakin muutoksia. Ennen pitkään saatamme esim. huomata, että sanajoukko pitää tallentaa tiedostoon. Jos tekisimme nämä muutokset Sanajoukkoon muuttamatta käyttöliittymän käyttävien metodien nimiä, ei käyttöliittymää tarvitsisi muuttaa mitenkään.

Oleellista on tässä se, että Sanajoukko-luokkaan tehdyt sisäiset muutokset eivät vaikuta luokkaan Käyttöliittymä. Tämä johtuu siitä, että käyttöliittymä käyttää sanajoukkoa sen tarjoamien metodien -- eli julkisten rajapintojen -- kautta.


## Uusien toiminnallisuuksien toteuttaminen: palindromit

Voi olla, että jatkossa ohjelmaa halutaan laajentaa siten, että `Sanajoukko`-luokan olisi osattava uusia asiota. Jos ohjelmassa haluttaisiin esimerkiksi tietää kuinka moni syötetyistä sanoista oli palindromi, voidaan sanajoukkoa laajentaa metodilla `palindromeja`.

 -->
## Changing the implementation of a class

We have arrived into a situation where the class 'Wordset' "encapsulates" an ArrayList. Is this reasonable? Perhaps. This is because we can make other changes to the class if we so desire, and before long we might arrive into a situation where the wordset has to be, for example, saved into a file. If we make all these changes inside the class Wordset without changing the names of the methods that the user interface uses, we don't have to modify the actual user interface at all.

The main point here is that changes made inside the class Wordset don't affect the class UserInterface. This is because the user interface uses Wordset through the methods that it provides -- these are called its public interfaces.


## Implementing new functionalities: palindroms

In the future, we might want to augment the program so that the class 'Wordset' offers some new functionalities. If, for example, we wanted to know how many of the entered words were palindroms, we could add a method called 'palindroms' into the program.


<!--
```java
public void kaynnista() {

    while (true) {
        System.out.print("Anna sana: ");
        String sana = lukija.nextLine();

        if (this.sanajoukko.sisaltaa(sana)) {
            break;
        }

        this.sanajoukko.lisaa(sana);
    }

    System.out.println("Annoit saman sanan uudestaan!");
    System.out.println("Sanoistasi " + this.sanajoukko.palindromeja() + " oli palindromeja");
}
``` -->
```java
public void start() {

    while (true) {
        System.out.print("Enter a word: ");
        String word = scanner.nextLine();

        if (this.wordset.contains(word)) {
            break;
        }

        this.wordset.add(word);
    }

    System.out.println("You gave the same word twice!");
    System.out.println(this.wordset.palindroms() + " of the words were palindroms.");
}
```

<!-- Käyttöliittymä säilyy siistinä ja palindromien laskeminen jää `Sanajoukko`-olion huoleksi. Metodin toteutus voisi olla esimerkiksi seuraavanlainen.
 -->
The user interface remains clean, because counting the palindroms is done inside the object 'Wordset'. The following is an example implementation of the method.

<!-- ```java
import java.util.ArrayList;

public class Sanajoukko {
    private ArrayList<String> sanat;

    public Sanajoukko() {
        this.sanat = new ArrayList<>();
    }

    public boolean sisaltaa(String sana) {
        return this.sanat.contains(sana);
    }

    public void lisaa(String sana) {
        this.sanat.add(sana);
    }

    public int palindromeja() {
        int lukumaara = 0;

        for (String sana: this.sanat) {
            if (onPalindromi(sana)) {
                lukumaara++;
            }
        }

        return lukumaara;
    }

    public boolean onPalindromi(String sana) {
        int loppu = sana.length() - 1;

        int i = 0;
        while (i < sana.length() / 2) {
            // metodi charAt palauttaa annetussa indeksissä olevan merkin
            // alkeistyyppisenä char-muuttujana
            if(sana.charAt(i) != sana.charAt(loppu - i)) {
                return false;
            }

            i++;
        }

        return true;
    }
}
``` -->
```java
import java.util.ArrayList;

public class Wordset {
    private ArrayList<String> words;

    public Wordset() {
        this.words = new ArrayList<>();
    }

    public boolean contains(String word) {
        return this.words.contains(word);
    }

    public void add(String word) {
        this.words.add(word);
    }

    public int palindroms() {
        int count = 0;

        for (String word: this.words) {
            if (isPalindrom(word)) {
                count++;
            }
        }

        return count;
    }

    public boolean isPalindrom(String word) {
        int end = word.length() - 1;

        int i = 0;
        while (i < word.length() / 2) {
            // method charAt returns the character at given index
            // as a simple variable
            if(word.charAt(i) != word.charAt(end - i)) {
                return false;
            }

            i++;
        }

        return true;
    }
}
```

<!-- Metodissa `palindromeja` käytetään apumetodia `onPalindromi`, joka tarkastaa onko sille parametrina annettu sana palindromi.
 -->
The method 'palindroms' uses the variable 'isPalindrom' to check whether the word that's given to it as a parameter is, in fact, a palindrom.

<!-- <text-box variant='hint' name='Uusiokäyttö'>

Kun ohjelmakoodin käsitteet on eriytetty omiksi luokikseen, voi niitä uusiokäyttää helposti muissa projekteissa. Esimerkiksi luokkaa `Sanajoukko` voisi käyttää yhtä hyvin graafisesta käyttöliittymästä, ja se voisi myös olla osa kännykässä olevaa sovellusta. Tämän lisäksi ohjelman toiminnan testaaminen on huomattavasti helpompaa silloin kun ohjelma on jaettu erillisiin käsitteisiin, joita kutakin voi käyttää myös omana itsenäisenä yksikkönään.

</text-box>
 -->
<text-box variant='hint' name='Recycling'>

When concepts have been separated into different classes in the code, recycling them and reusing them in other projects becomes easy. For example, the class 'Wordset' could be well be used in a graphical user interface, and it could also part of a mobile phone application. In addition, testing the program is much easier when it has been divided into several concepts, each of which has its own separate logic and can function alone as a unit.

</text-box>

<!-- ## Neuvoja ohjelmointiin

Yllä kuvatussa laajemmassa esimerkissä noudatettiin seuraavia neuvoja.


-  Etene pieni askel kerrallaan
    -  Yritä pilkkoa ongelma osaongelmiin ja **ratkaise vain yksi osaongelma kerrallaan**
    -  Testaa aina että ohjelma on etenemässä oikeaan suuntaan eli että osaongelman ratkaisu meni oikein
    -  Tunnista ehdot, minkä tapauksessa ohjelman tulee toimia eri tavalla. Esimerkiksi yllä tarkistus, jolla katsotaan onko sana jo syötetty, johtaa erilaiseen toiminnallisuuden.

-  Kirjoita mahdollisimman "siistiä" koodia
    -  sisennä koodi
    -  käytä kuvaavia muuttujien ja metodien nimiä
    -  älä tee liian pitkiä metodeja, edes mainia
    -  tee yhdessä metodissa vaan yksi asia
    -  **poista koodistasi kaikki copy-paste**
    -  korvaa koodisi "huonot" ja epäsiistit osat siistillä koodilla

- Astu tarvittaessa askel taaksepäin ja mieti kokonaisuutta. Jos ohjelma ei toimi, voi olla hyvä idea palata aiemmin toimineeseen tilaan. Käänteisesti voidaan sanoa, että rikkinäinen ohjelma korjaantuu harvemmin lisäämällä siihen lisää koodia.

Ohjelmoijat noudattavat näitä käytänteitä sen takia että ohjelmointi olisi helpompaa. Käytänteiden noudattaminen tekee myös ohjelmien lukemisesta, ylläpitämisestä ja muokkaamisesta helpompaa muille.
 -->
## Programming tips

In the larger example above, we were following the advice given here.


-  Proceed with small steps
    -  Try to separate the program into several sub-problems and **work on only one sub-problem at a time**
    -  Always test that the program code is advancing in the right direction, in other words: test that the solution to the sub-problem is correct
    -  Recognize the conditions that require the program to work differently. In the example above, we needed a different functionality to test whether a word had been already entered before.

-  Write as "clean" code as possible
    -  Indent your code
    -  Use descriptive method and variable names
    -  Don't make your methods too long, not even the main method
    -  Do only one thing inside one method
    -  **Remove all copy-paste code**
    -  Replace the "bad" and unclean parts of your code with clean code

-  If needed, take a step back and assess the program as a whole. If it doesn't work, it might be a good idea to return into a previous state where the code still worked. As a corollary, we might say that a program that's broken is rarely fixed by adding more code to it.

Programmers follow these conventions so that programming can be made easier. Following them also makes it easier to read programs, to keep them up, and to edit them in teams.

<programming-exercise name='Dictionary (4 parts)' tmcname='part06-Part06_09.Dictionary'>

Tehtäväpohjassa on valmiiksi annettuna luokka `Sanakirja`, joka tarjoaa toiminnallisuuden sanojen ja niiden käännösten tallentamiseen. Vaikka luokan sisäisessä totetuksessa on asioita, joita kurssilla ei ole käsitelty, on sen käyttö suoraviivaista:

```java
Sanakirja kirja = new Sanakirja();
kirja.lisaa("yksi", "one");
kirja.lisaa("kaksi", "two");

System.out.println(kirja.kaanna("yksi"));
System.out.println(kirja.kaanna("kaksi"));
System.out.println(kirja.kaanna("kolme"));
```

<sample-output>

one
two
null

</sample-output>

Tässä tehtävässä toteutat luokkaa `Sanakirja` hyödyntävän tekstikäyttöliittymän.


<h2>Tekstikäyttöliittymän käynnistys ja lopetus</h2>

Toteuta luokka `Tekstikayttoliittyma`, joka saa konstruktorin parametrina `Scanner`-olion sekä `Sanakirja`-olion. Lisää tämän jälkeen luokalle metodi `public void kaynnista()`. Metodin tulee toimia seuraavalla tavalla:

1. Metodi kysyy käyttäjältä komentoa.
2. Mikäli komento on `lopeta`, tekstikäyttöliittymä tulostaa merkkijonon "Hei hei!" ja metodin `kaynnista` suoritus päättyy.
3. Muulloin, tekstikäyttöliittymä tulostaa viestin "Tuntematon komento", jonka jälkeen metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1.


```java
Scanner lukija = new Scanner(System.in);
Sanakirja kirja = new Sanakirja();

Tekstikayttoliittyma kayttoliittyma = new Tekstikayttoliittyma(lukija, kirja);
kayttoliittyma.kaynnista();
```

<sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Tuntematon komento
Komento: **lopeta**
Hei hei!

</sample-output>


<h2>Käännösten lisääminen</h2>

Muokkaa metodia `public void kaynnista()` siten, että se toimii seuraavalla tavalla:

1. Metodi kysyy käyttäjältä komentoa.
2. Mikäli komento on `lopeta`, tekstikäyttöliittymä tulostaa merkkijonon "Hei hei!" ja metodin `kaynnista` suoritus päättyy.
3. Mikäli komento on `lisaa`, tekstikäyttöliittymä kysyy käyttäjältä sanaa ja käännöstä, kumpaakin omalla rivillään. Tämän jälkeen sanat lisätään sanakirjaan ja metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1.
4. Muulloin, tekstikäyttöliittymä tulostaa viestin "Tuntematon komento", jonka jälkeen metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1.


<sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Sana: **hauki**
Käännös: **pike**
Komento: **muuta**
Tuntematon komento
Komento: **lopeta**
Hei hei!

</sample-output>

Yllä kuvatussa esimerkissä sanakirja-olioon lisätään sana "hauki" sekä sen käännös "pike". Sanakirjaa voisi tällöin käyttöliittymästä poistumisen jälkeen käyttää seuraavasti:

```java
Scanner lukija = new Scanner(System.in);
Sanakirja kirja = new Sanakirja();

Tekstikayttoliittyma kayttoliittyma = new Tekstikayttoliittyma(lukija, kirja);
kayttoliittyma.kaynnista();
System.out.println(kirja.kaanna("hauki")); // tulostaa merkkijonon "pike"
```


<h2>Sanan kääntäminen</h2>


Muokkaa metodia `public void kaynnista()` siten, että se toimii seuraavalla tavalla:

1. Metodi kysyy käyttäjältä komentoa.
2. Mikäli komento on `lopeta`, tekstikäyttöliittymä tulostaa merkkijonon "Hei hei!" ja metodin `kaynnista` suoritus päättyy.
3. Mikäli komento on `lisaa`, tekstikäyttöliittymä kysyy käyttäjältä sanaa ja käännöstä, kumpaakin omalla rivillään. Tämän jälkeen sanat lisätään sanakirjaan ja metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1.
4. Mikäli komento on `hae`, tekstikäyttöliittymä kysyy käyttäjältä käännettävää sanaa. Tämän jälkeen tekstikäyttöliittymä tulostaa sanan käännöksen ja metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1.
5. Muulloin, tekstikäyttöliittymä tulostaa viestin "Tuntematon komento", jonka jälkeen metodi jatkaa kysymällä käyttäjältä komentoa eli kohdasta 1.


<sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Sana: **hauki**
Käännös: **pike**
Komento: **muuta**
Tuntematon komento
Komento: **hae**
Haettava: **hauki**
Käännös: pike
Komento: **hae**
Haettava: **nauris**
Käännös: null
Komento: **lopeta**
Hei hei!

</sample-output>


<h2>Käännöksen siistiminen</h2>

Muokkaa tekstikäyttöliittymän hakutoiminnallisuutta siten, että mikäli sanaa ei löydy (eli sanakirja palauttaa `null`-viitteen), tekstikäyttöliittymä tulostaa viestin "Sanaa (haettava) ei löydy".

<sample-output>

Komento: **jotain**
Tuntematon komento
Komento: **lisaa**
Sana: **hauki**
Käännös: **pike**
Komento: **muuta**
Tuntematon komento
Komento: **hae**
Haettava: **hauki**
Käännös: pike
Komento: **hae**
Haettava: **nauris**
Sanaa nauris ei löydy
Komento: **lopeta**
Hei hei!

</sample-output>


</programming-exercise>


<programming-exercise name='Tasklist (2 parts)' tmcname='part06-Part06_10.Tasklist'>

Tässä tehtävässä tehdään sovellus tehtävälistan luomiseen ja käsittelyyn. Lopullinen sovellus tulee toimimaan seuraavalla tavalla.

<sample-output>

Komento: **lisaa**
Tehtävä: **käy kaupassa**
Komento: **lisaa**
Tehtävä: **imuroi**
Komento: **listaa**
1: käy kaupassa
2: imuroi
Komento: **valmis**
Mikä valmistui? **2**
Tehtävä käy kaupassa tehty
Komento: **listaa**
1: käy kaupassa
Komento: **lisaa**
Tehtävä: **ohjelmoi**
Komento: **listaa**
1: käy kaupassa
2: ohjelmoi
Komento: **lopeta**

</sample-output>

Tehdään sovellus osissa.

<h2>Tehtävälista</h2>

Luo luokka `Tehtavalista`. Luokalla tulee olla parametriton konstruktori sekä seuraavat metodit:

- `public void lisaa(String tehtava)` - lisää tehtävälistalle parametrina annetun tehtävän.
- `public void tulosta()` - tulostaa tehtävät. Tulostuksessa jokaiselle tehtävällä on myös numero -- käytä tässä tehtävän indeksiä (+1).
- `public void poista(int numero)` - poistaa annettua numeroa vastaavan tehtävän; numero liittyy tulostuksessa nähtyyn tehtävän numeroon.

```java
Tehtavalista lista = new Tehtavalista();
lista.lisaa("lue kurssimateriaalia");
lista.lisaa("katso uusin fool us");
lista.lisaa("ota rennosti");

lista.tulosta();
lista.poista(2);

System.out.println();
lista.tulosta();
```

<sample-output>

1: lue kurssimateriaalia
2: katso uusin fool us
3: ota rennosti

1: lue kurssimateriaalia
2: ota rennosti

</sample-output>

**Huom!** Voit olettaa, että metodille `poista` syötetään oikea tehtävää vastaava numero. Metodin tarvitsee toimia oikein vain kerran kunkin tulostuskutsun jälkeen.

Toinen esimerkki:

```java
Tehtavalista lista = new Tehtavalista();
lista.lisaa("lue kurssimateriaalia");
lista.lisaa("katso uusin fool us");
lista.lisaa("ota rennosti");
lista.tulosta();
lista.poista(2);
lista.tulosta();
lista.lisaa("osta rusinoita");
lista.tulosta();
lista.poista(1);
lista.poista(1);
lista.tulosta();
```

<sample-output>

1: lue kurssimateriaalia
2: katso uusin fool us
3: ota rennosti
1: lue kurssimateriaalia
2: ota rennosti
1: lue kurssimateriaalia
2: ota rennosti
3: osta rusinoita
1: osta rusinoita

</sample-output>


<h2>Käyttöliittymä</h2>

Toteuta seuraavaksi luokka `Kayttoliittyma`. Luokalla `Kayttoliittyma` tulee olla kaksiparametrinen konstruktori. Ensimmäisenä parametrina annetaan luokan `Tehtavalista` ilmentymä ja toisena parametrina luokan `Scanner` ilmentymä. Konstruktorin lisäksi luokalla tulee olla metodi `public void kaynnista()`, joka käynnistää tekstikäyttöliittymän. Tekstikäyttöliittymä toteutetaan ikuisen toistolauseen (`while-true`) avulla, ja sen tulee tarjota seuraavat komennot:

- Komento `lopeta` lopettaa toistolauseen suorituksen, jonka jälkeen ohjelman suoritus palaa `kaynnista`-metodista.
- Komento `lisaa` kysyy käyttäjältä lisättävää tehtävää. Kun käyttäjä syöttää lisättävän tehtävän, tulee se lisätä tehtävälistalle.
- Komento `listaa` tulostaa kaikki tehtävälistalla olevat tehtävät.
- Komento `poista` kysyy käyttäjältä poistettavan tehtävän tunnusta ja poistaa käyttäjän syöttämää tunnusta vastaavan tehtävän tehtävälistalta.

Alla on esimerkki sovelluksen toiminnasta.

<sample-output>

Komento: **lisaa**
Lisättävä: **kirjoita essee**
Komento: **lisaa**
Lisättävä: **lue kirja**
Komento: **listaa**
1: kirjoita essee
2: lue kirja
Komento: **poista**
Mikä poistetaan? **1**
Komento: **listaa**
1: lue kirja
Komento: **poista**
Mikä poistetaan? **1**
Komento: **listaa**
Komento: **lisaa**
Lisättävä: **lopeta**
Komento: **listaa**
1: lopeta
Komento: **lopeta**

</sample-output>

Huom! Käyttöliittymän tulee käyttää sille parametrina annettua tehtävälistaa ja Scanneria.

</programming-exercise>


<!-- ## Sovelluksesta osakokonaisuuksiin


Tarkastellaan ohjelmaa, joka kysyy käyttäjältä koepisteitä, muuntaa ne arvosanoiksi, ja lopulta tulostaa kurssin arvosanajakauman tähtinä. Ohjelma lopettaa lukemisen kun käyttäjä syöttää tyhjän merkkijonon. Ohjelman käyttö näyttää seuraavalta:
 -->
Let's examine a program that asks the user to input exam points and turns them into grades. Finally, the program prints the distribution of the grades as stars. The program stops reading inputs when the user inputs an empty string. An example program looks as follows:
<!--
<sample-output>

Syötä koepisteet: **91**
Syötä koepisteet: **98**
Syötä koepisteet: **103**
Epäkelpo luku.
Syötä koepisteet: **90**
Syötä koepisteet: **89**
Syötä koepisteet: **89**
Syötä koepisteet: **88**
Syötä koepisteet: **72**
Syötä koepisteet: **54**
Syötä koepisteet: **55**
Syötä koepisteet: **51**
Syötä koepisteet: **49**
Syötä koepisteet: **48**
Syötä koepisteet:

5: \*\*\*
4: \*\*\*
3: \*
2:
1: \*\*\*
0: \*\*

</sample-output> -->
<sample-output>

Input exam points: **91**
Input exam points: **98**
Input exam points: **103**
Invalid number.
Input exam points: **90**
Input exam points: **89**
Input exam points: **89**
Input exam points: **88**
Input exam points: **72**
Input exam points: **54**
Input exam points: **55**
Input exam points: **51**
Input exam points: **49**
Input exam points: **48**
Input exam points:

5: \*\*\*
4: \*\*\*
3: \*
2:
1: \*\*\*
0: \*\*

</sample-output>

<!-- Kuten lähes kaikki ohjelmat, ohjelman voi kirjoittaa yhtenä kokonaisuutena mainiin. Eräs mahdollinen toteutus on seuraavanlainen.
 -->
As almost all programs, this program can be written into main as one entity. Here is one possibility.

<!-- ```java
import java.util.ArrayList;
import java.util.Scanner;

public class Ohjelma {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);

        ArrayList<Integer> arvosanat = new ArrayList<>();

        while (true) {
            System.out.print("Syötä koepisteet: ");
            String luettu = lukija.nextLine();
            if (luettu.equals("")) {
                break;
            }

            int pisteet = Integer.valueOf(luettu);

            if (pisteet < 0 || pisteet > 100) {
                System.out.println("Epäkelpo luku.");
                continue;
            }

            int arvosana = 0;
            if (pisteet < 50) {
                arvosana = 0;
            } else if (pisteet < 60) {
                arvosana = 1;
            } else if (pisteet < 70) {
                arvosana = 2;
            } else if (pisteet < 80) {
                arvosana = 3;
            } else if (pisteet < 90) {
                arvosana = 4;
            } else {
                arvosana = 5;
            }

            arvosanat.add(arvosana);
        }

        System.out.println("");
        int arvosana = 5;
        while (arvosana >= 0) {
            int tahtia = 0;
            for (int saatu: arvosanat) {
                if (saatu == arvosana) {
                    tahtia++;
                }
            }

            System.out.print(arvosana + ": ");
            while (tahtia > 0) {
                System.out.print("*");
                tahtia--;
            }
            System.out.println("");

            arvosana = arvosana - 1;
        }
    }
}
``` -->
```java
import java.util.ArrayList;
import java.util.Scanner;

public class Program {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        ArrayList<Integer> grades = new ArrayList<>();

        while (true) {
            System.out.print("Input grades: ");
            String input = scanner.nextLine();
            if (input.equals("")) {
                break;
            }

            int score = Integer.valueOf(input);

            if (score < 0 || score > 100) {
                System.out.println("Invalid number.");
                continue;
            }

            int grade = 0;
            if (score < 50) {
                grade = 0;
            } else if (score < 60) {
                grade = 1;
            } else if (score < 70) {
                grade = 2;
            } else if (score < 80) {
                grade = 3;
            } else if (score < 90) {
                grade = 4;
            } else {
                grade = 5;
            }

            grades.add(grade);
        }

        System.out.println("");
        int grade = 5;
        while (grade >= 0) {
            int stars = 0;
            for (int received: grades) {
                if (received == grade) {
                    stars++;
                }
            }

            System.out.print(grade + ": ");
            while (stars > 0) {
                System.out.print("*");
                stars--;
            }
            System.out.println("");

            grade = grade - 1;
        }
    }
}
```

<!-- Pilkotaan ohjelma pienempiin osiin. Ohjelman pilkkominen tapahtuu tunnistamalla ohjelmasta vastuualueita. Arvosanojen kirjanpito, mukaanlukien pisteiden muunnos arvosanoiksi, voisi olla erillisen luokan vastuulla. Tämän lisäksi käyttöliittymälle voidaan luoda oma luokkansa.
 -->
Let's separate the program into smaller chunks. This can be done by identifying several discrete areas of responsibility within the program. Keeping track of grades, including converting scores into grades, could be done inside a different class. In addition, we could create a new class for the user interface.

<!-- ### Sovelluslogikkka

Sovelluslogiikka sisältää ohjelman toiminnan kannalta oleellisen osat kuten tiedon säilöntätoiminnallisuuden. Edellisestä esimerkistä voidaan tunnistaa arvosanojen säilömiseen tarvittava toiminnallisuus. Eriytetään luokka `Arvosanarekisteri`, jonka vastuulle tulee kirjanpito arvosanojen lukumääristä. Arvosanarekisteriin voidaan lisätä arvosana pisteiden perusteella, jonka lisäksi arvosanarekisteristä voi kysyä kuinka moni on saanut tietyn arvosanan.

Luokka voi näyttää esimerkiksi seuraavalta.
 -->
### Program logic

Program logic includes parts that are crucial for the execution of the program, like functionalities that store information, for example. From the previous example, we can separate the parts that store grade information. From these we can make a class called 'GradeRegister', which is responsible for keeping track of the numbers of different grades students have received. In the register, we can add grades according to scores. In addition, we can use the register to ask how many people have received a certain grade.

An example class follows.

<!-- ```java
import java.util.ArrayList;

public class Arvosanarekisteri {

    private ArrayList<Integer> arvosanat;

    public Arvosanarekisteri() {
        this.arvosanat = new ArrayList<>();
    }

    public void lisaaArvosanaPisteidenPerusteella(int pisteet) {
        this.arvosanat.add(pisteetArvosanaksi(pisteet));
    }

    public int montakoSaanutArvosanan(int arvosana) {
        int lkm = 0;
        for (int saatu: this.arvosanat) {
            if (saatu == arvosana) {
                lkm++;
            }
        }

        return lkm;
    }

    public static int pisteetArvosanaksi(int pisteet) {

        int arvosana = 0;
        if (pisteet < 50) {
            arvosana = 0;
        } else if (pisteet < 60) {
            arvosana = 1;
        } else if (pisteet < 70) {
            arvosana = 2;
        } else if (pisteet < 80) {
            arvosana = 3;
        } else if (pisteet < 90) {
            arvosana = 4;
        } else {
            arvosana = 5;
        }

        return arvosana;
    }
}
``` -->
```java
import java.util.ArrayList;

public class GradeRegister {

    private ArrayList<Integer> grades;

    public GradeRegister() {
        this.grades = new ArrayList<>();
    }

    public void addGradeAccordingToPoints(int points) {
        this.grades.add(pointsToGrades(points));
    }

    public int howManyReceivedGrade(int grade) {
        int count = 0;
        for (int received: this.grades) {
            if (received == grade) {
                count++;
            }
        }

        return count;
    }

    public static int pointsToGrades(int points) {

        int grade = 0;
        if (points < 50) {
            grade = 0;
        } else if (points < 60) {
            grade = 1;
        } else if (points < 70) {
            grade = 2;
        } else if (points < 80) {
            grade = 3;
        } else if (points < 90) {
            grade = 4;
        } else {
            grade = 5;
        }

        return grade;
    }
}
```

<!-- Kun arvosanarekisteri on eriytetty omaksi luokakseen, voidaan siihen liittyvä toiminnallisuus poistaa pääohjelmastamme. Pääohjelman muoto on nyt seuraavanlainen.
 -->
When the grade register has been separated into a class, we can remove the functionality associated with it from our main program. The main program now looks like this.

```java
import java.util.Scanner;

public class Ohjelma {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);

        Arvosanarekisteri rekisteri = new Arvosanarekisteri();

        while (true) {
            System.out.print("Syötä koepisteet: ");
            String luettu = lukija.nextLine();
            if (luettu.equals("")) {
                break;
            }

            int pisteet = Integer.valueOf(luettu);

            if (pisteet < 0 || pisteet > 100) {
                System.out.println("Epäkelpo luku.");
                continue;
            }

            rekisteri.lisaaArvosanaPisteidenPerusteella(pisteet);
        }

        System.out.println("");
        int arvosana = 5;
        while (arvosana >= 0) {
            int tahtia = rekisteri.montakoSaanutArvosanan(arvosana);
            System.out.print(arvosana + ": ");
            while (tahtia > 0) {
                System.out.print("*");
                tahtia--;
            }
            System.out.println("");

            arvosana = arvosana - 1;
        }
    }
}
```

Sovelluslogiikan eriyttämisestä tulee merkittävä hyöty ohjelman ylläpidettävyyden kannalta. Koska sovelluslogiikka -- tässä Arvosanarekisteri -- on erillinen luokka, voidaan sitä myös testata ohjelmasta erillisenä osana. Luokan `Arvosanarekisteri` voisi halutessaan kopioida myös muihin ohjelmiinsa. Alla on esimerkki yksinkertaisesta luokan `Arvosanarekisteri` manuaalsesta testaamisesta -- tämä kokeilu huomioi vain pienen osan rekisterin toiminnallisuudesta.

```java
Arvosanarekisteri rekisteri = new Arvosanarekisteri();
rekisteri.lisaaArvosanaPisteidenPerusteella(51);
rekisteri.lisaaArvosanaPisteidenPerusteella(50);
rekisteri.lisaaArvosanaPisteidenPerusteella(49);

System.out.println("Arvosanan 0 saaneita (pitäisi olla 1): " + rekisteri.montakoSaanutArvosanan(0));
System.out.println("Arvosanan 1 saaneita (pitäisi olla 2): " + rekisteri.montakoSaanutArvosanan(2));
```


### Käyttöliittymä

Käyttöliittymä on tyypillisesti sovelluskohtainen. Luodaan luokka `Kayttoliittyma` ja eriytetään se pääohjelmasta. Käyttöliittymälle annetaan parametrina arvosanarekisteri, jota käytetään arvosanojen säilömiseen, ja Scanner-olio, jota käytetään syötteen lukemiseen.

Kun käytössämme on käyttöliittymä, muodostuu ohjelman käynnistävästä pääohjelmasta hyvin selkeä.

```java
import java.util.Scanner;

public class Ohjelma {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);

        Arvosanarekisteri rekisteri = new Arvosanarekisteri();

        Kayttoliittyma kayttoliittyma = new Kayttoliittyma(rekisteri, lukija);
        kayttoliittyma.kaynnista();
    }
}
```

Tarkastellaan käyttöliittymän toteutusta. Käyttöliittymässä on oleellisesti kaksi osaa: pisteiden lukeminen sekä arvosanajakauman tulostaminen.

```java
import java.util.Scanner;

public class Kayttoliittyma {

    private Arvosanarekisteri rekisteri;
    private Scanner lukija;

    public Kayttoliittyma(Arvosanarekisteri rekisteri, Scanner lukija) {
        this.rekisteri = rekisteri;
        this.lukija = lukija;
    }

    public void kaynnista() {
        lueKoepisteet();
        System.out.println("");
        tulostaArvosanajakauma();
    }

    public void lueKoepisteet() {
    }

    public void tulostaArvosanajakauma() {
    }
}
```

Voimme kopioida koepisteiden lukemisen sekä arvosanajakauman tulostamisen lähes suoraan aiemmasta pääohjelmastamme. Alla olevassa ohjelmassa osat on kopioitu aiemmasta pääohjelmasta, jonka lisäksi tähtien tulostukseen on luotu erillinen metodi -- tämä selkiyttää arvosanojen tulostamiseen käytettävää metodia.

```java
import java.util.Scanner;

public class Kayttoliittyma {

    private Arvosanarekisteri rekisteri;
    private Scanner lukija;

    public Kayttoliittyma(Arvosanarekisteri rekisteri, Scanner lukija) {
        this.rekisteri = rekisteri;
        this.lukija = lukija;
    }

    public void kaynnista() {
        lueKoepisteet();
        System.out.println("");
        tulostaArvosanajakauma();
    }

    public void lueKoepisteet() {
        while (true) {
            System.out.print("Syötä koepisteet: ");
            String luettu = lukija.nextLine();
            if (luettu.equals("")) {
                break;
            }

            int pisteet = Integer.valueOf(luettu);

            if (pisteet < 0 || pisteet > 100) {
                System.out.println("Epäkelpo luku.");
                continue;
            }

            this.rekisteri.lisaaArvosanaPisteidenPerusteella(pisteet);
        }
    }

    public void tulostaArvosanajakauma() {
        int arvosana = 5;
        while (arvosana >= 0) {
            int tahtia = rekisteri.montakoSaanutArvosanan(arvosana);
            System.out.print(arvosana + ": ");
            tulostaTahtia(tahtia);
            System.out.println("");

            arvosana = arvosana - 1;
        }
    }

    public static void tulostaTahtia(int tahtia) {
        while (tahtia > 0) {
            System.out.print("*");
            tahtia--;
        }
    }
}
```


<programming-exercise name='Keskiarvot (3 osaa)' tmcname='osa06-Osa06_11.Keskiarvot'>

Tehtäväpohjassa on edellisessä esimerkissä rakennettu arvosanojen tallentamiseen tarkoitettu ohjelma. Tässä tehtävässä täydennät luokkaa `Arvosanarekisteri` siten, että se tarjoaa toiminnallisuuden arvosanojen ja koepisteiden keskiarvon laskemiseen.


<h2>Arvosanojen keskiarvo</h2>

Lisää luokalle `Arvosanarekisteri` metodi `public double arvosanojenKeskiarvo()`, joka palauttaa arvosanojen keskiarvon. Mikäli arvosanarekisterissä ei ole yhtäkään arvosanaa, tulee metodin palauttaa luku `-1`.  Laske arvosanojen keskiarvo `arvosanat`-listaa hyödyntäen.

Käyttöesimerkki:

```java
Arvosanarekisteri rekisteri = new Arvosanarekisteri();
rekisteri.lisaaArvosanaPisteidenPerusteella(93);
rekisteri.lisaaArvosanaPisteidenPerusteella(91);
rekisteri.lisaaArvosanaPisteidenPerusteella(92);
rekisteri.lisaaArvosanaPisteidenPerusteella(88);

System.out.println(rekisteri.arvosanojenKeskiarvo());
```

<sample-output>

4.75

</sample-output>



<h2>Koepisteiden keskiarvo</h2>

Lisää luokalle `Arvosanarekisteri` uusi oliomuuttuja lista, johon lisäät koepisteitä aina kun luokkaa käyttävä ohjelma kutsuu metodia `lisaaArvosanaPisteidenPerusteella`. Lisää tämän jälkeen luokalle metodi `public double koepisteidenKeskiarvo()`, joka laskee ja palauttaa koepisteiden keskiarvon. Mikäli arvosanarekisteriin ei ole lisätty yhtäkään koepistettä, tulee metodin palauttaa luku `-1`.

Käyttöesimerkki:

```java
Arvosanarekisteri rekisteri = new Arvosanarekisteri();
rekisteri.lisaaArvosanaPisteidenPerusteella(93);
rekisteri.lisaaArvosanaPisteidenPerusteella(91);
rekisteri.lisaaArvosanaPisteidenPerusteella(92);

System.out.println(rekisteri.koepisteidenKeskiarvo());
```

<sample-output>

92.0

</sample-output>


<h2>Tulostukset käyttöliittymään</h2>

Lisää lopulta edellä toteutetut metodit osaksi käyttöliittymää. Kun sovellus tulostaa arvosanajakauman, tulee sovelluksen tulostaa myös pisteiden ja arvosanojen keskiarvo.

<sample-output>

Syötä koepisteet: **82**
Syötä koepisteet: **83**
Syötä koepisteet: **96**
Syötä koepisteet: **51**
Syötä koepisteet: **48**
Syötä koepisteet: **56**
Syötä koepisteet: **61**
Syötä koepisteet:

5: \*
4: \*\*
3:
2: \*
1: \*\*
0: \*
Koepisteiden keskiarvo: 68.14285714285714
Arvosanojen keskiarvo: 2.4285714285714284

</sample-output>

</programming-exercise>


<programming-exercise name='Sovellus osiin (2 osaa)' tmcname='osa06-Osa06_12.SovellusOsiin'>

Tehtäväpohjassa on valmiina seuraava "mainiin" kirjoitettu sovellus.

```java
Scanner lukija = new Scanner(System.in);
ArrayList<String> vitsit = new ArrayList<>();
System.out.println("Voihan vitsi!");

while (true) {
    System.out.println("Komennot:");
    System.out.println(" 1 - lisää vitsi");
    System.out.println(" 2 - arvo vitsi");
    System.out.println(" 3 - listaa vitsit");
    System.out.println(" X - lopeta");

    String komento = lukija.nextLine();

    if (komento.equals("X")) {
        break;
    }

    if (komento.equals("1")) {
        System.out.println("Kirjoita lisättävä vitsi:");
        String vitsi = lukija.nextLine();
        vitsit.add(vitsi);
    } else if (komento.equals("2")) {
        System.out.println("Arvotaan vitsi.");

        if (vitsit.isEmpty()) {
            System.out.println("Vitsit vähissä.");
        } else {
            Random arpa = new Random();
            int indeksi = arpa.nextInt(vitsit.size());
            System.out.println(vitsit.get(indeksi));
        }

    } else if (komento.equals("3")) {
        System.out.println("Tulostetaan vitsit.");
        for (String vitsi : vitsit) {
            System.out.println(vitsi);
        }
    }
}
```

Sovellus on käytännössä vitsipankki. Vitsipankkiin voi lisätä vitsejä, vitsipankista voi arpoa vitsejä, ja vitsipankissa olevat vitsit voidaan tulostaa. Tässä tehtävässä sovellus pilkotaan osiin ohjatusti.

<h2>Vitsipankki</h2>

Luo luokka `Vitsipankki` ja siirrä sinne vitsien hallinnointiin liittyvä toiminnallisuus. Luokalla tulee olla parametriton konstruktori sekä seuraavat metodit:

- `public void lisaaVitsi(String vitsi)` - lisää vitsin vitsipankkiin.
- `public String arvoVitsi()` - arpoo vitsipankin vitseistä yhden vitsin ja palauttaa sen. Mikäli vitsipankissa ei ole vitsejä, palauttaa merkkijonon "Vitsit vähissä.".
- `public void tulostaVitsit()` - tulostaa vitsipankissa olevat vitsit.

Esimerkki luokan käytöstä:

```java
Vitsipankki pankki = new Vitsipankki();
pankki.lisaaVitsi("Mikä on punaista ja tuoksuu siniselle maalille? - Punainen maali.");
pankki.lisaaVitsi("Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.");

System.out.println("Arvotaan vitsejä:");
for (int i = 0; i < 5; i++) {
    System.out.println(pankki.arvoVitsi());
}

System.out.println("");
System.out.println("Tulostetaan vitsit:");
pankki.tulostaVitsit();
```

Alla ohjelman mahdollinen tulostus. Huomaa, että arvotut vitsit eivät todennäköisesti tulostu alla kuvatun esimerkin mukaisesti.

<sample-output>

Arvotaan vitsejä:
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.
Mikä on punaista ja tuoksuu siniselle maalille? - Punainen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.

Tulostetaan vitsit:
Mikä on punaista ja tuoksuu siniselle maalille? - Punainen maali.
Mikä on sinistä ja tuoksuu punaiselle maalille? - Sininen maali.

</sample-output>


<h2>Käyttöliittymä</h2>

Luo luokka `Kayttoliittyma` ja siirrä sinne sovelluksen käyttöliittymätoiminnallisuus. Luokalla tulee olla kaksiparametrinen konstruktori. Ensimmäisenä parametrina annetaan Vitsipankki-luokan ilmentymä, ja toisena parametrina Scanner-luokan ilmentymä. Tämän lisäksi luokalla tulee olla metodi `public void kaynnista()`, joka käynnistää käyttöliittymän.

Käyttöliittymän tulee tarjota seuraavat komennot:

- `X` - lopettaminen: poistuu metodista `kaynnista`.
- `1` - lisääminen: kysyy käyttäjältä vitsipankkiin lisättävää vitsiä ja lisää käyttäjän syöttämän vitsin vitsipankkiin.
- `2` - arpominen: arpoo vitsipankista vitsin ja tulostaa sen. Mikäli vitsipankissa ei ole vitsejä, tulostaa merkkijonon "Vitsit vähissä.".
- `3` - tulostaminen: tulostaa kaikki vitsipankissa olevat vitsit.

Esimerkki käyttöliittymän käytöstä:

```java
Vitsipankki pankki = new Vitsipankki();
Scanner lukija = new Scanner(System.in);

Kayttoliittyma liittyma = new Kayttoliittyma(pankki, lukija);
liittyma.kaynnista();
```

<sample-output>

Komennot:
 1 - lisää vitsi
 2 - arvo vitsi
 3 - listaa vitsit
 X - lopeta
**1**
Kirjoita lisättävä vitsi:
**Did you hear about the claustrophobic astronaut? -- He just needed a little space.**
Komennot:
 1 - lisää vitsi
 2 - arvo vitsi
 3 - listaa vitsit
 X - lopeta
**3**
Tulostetaan vitsit.
Did you hear about the claustrophobic astronaut? -- He just needed a little space.
Komennot:
 1 - lisää vitsi
 2 - arvo vitsi
 3 - listaa vitsit
 X - lopeta
**X**

</sample-output>

</programming-exercise>

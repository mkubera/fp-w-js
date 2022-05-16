# Funkcyjne programowanie w JavaScript

# Spis tresci

- [Czym jest JavaScript w kontekscie funkcyjnego programowania](#czym-jest-js-w-kontekscie-fp)
  - [Ramda docs - jak czytac anotacje](#ramda-docs---jak-czytac-anotacje)
- [Koncepty funkcyjnego programowania uzywane w JavaScript](#koncepty-funkcyjnego-programowania-uzywane-w-js)
  - [Pure functions](#pure-functions)
  - [Immutability](#immutability)
  - [First-class functions](#first-class-functions)
  - [Higher-order functions](#higher-order-functions)
  - [Declarative code](#declarative-code)
  - [Currying](#currying)
  - [Popularne operacje na kolekcjach](#popularne-operacje-na-kolekcjach)
  - [Pipe oraz Compose](#pipe-oraz-compose)
- [Przykladowe uzycia](#przykladowe-uzycia)
  - [Wybieranie z listy](#wybieranie-z-listy)
  - [Dodawanie do Listy](#dodawanie-do-listy)
  - [Usuwanie z Listy](#usuwanie-z-listy)
  - [Update'owanie Listy](#updateowanie-listy)
- [Cwiczenia](#cwiczenia)
- [Przykłady użycia funkcyjnego paradygmatu w "mainstreamie" (np. we frameworku React)](#przyklady-uzycia-funkcyjnego-paradygmatu)
- [Cwiczenia 2](#cwiczenia-2)
- [Uzyteczne linki](#uzyteczne-linki)

# Czym jest JS w kontekscie FP

JavaScript jest paradygmatycznym jezykiem, ktory ma spora ilosc funkcyjnych konceptow zaimplementowana w sobie. Funkcje sa funkcjami pierwszego typu (parametry moga byc funkcjami, funkcje moga byc zwracane jako [closures](<https://en.wikipedia.org/wiki/Closure_(computer_programming)#Anonymous_functions>). Arrays maja metody, ktore sa funkcjami wyzszego rzedu (`map`, `filter`, `reduce`, itp.), ktore sa klasycznie znajdowane w jezykach funkcyjnych. Niektore typy danych sa niemutowalne 'by default' (w programowaniu funkcyjnym wszystkie dane sa niemutowalne (zazwyczaj :))). Mozemy tworzyc czyste funkcje (pure functions) w JS, i to z uzyciem latwej skladni (`=>`, czyli arrow functions). I kilka innych przyjemnosci, o ktorych wiecej pozniej.

Czy JS jest jezykiem funkcyjnym? Zdecydowanie nie. Ale JS ma w sobie paradygmat funkcyjny (podobnie jak Ruby, Python, Scala, i kilka innych popularnych jezykow). Dzis bedziemy ogarniac FP w JS na dwa sposoby.

1. Poslugujac sie tym, co vanilla JS ma do zaoferowania.
1. Poslugujac sie [Ramda.js](https://ramdajs.com/), ktora jest biblioteka najbardziej zblizona do 'purely functional languages' takich jak [Haskell](https://www.haskell.org/) czy [Elm](http://elm-lang.org/). Strona Haskell ma swietne wyjasnienie, czym jest "purely", ktore (licze 😀) da niektorym z Was ten jakze wazny "gotcha", a moze nawet "WTFFF???" "moment". A wiec!
   > Kazda funkcja w Haskell jest funkcja w sensie matematycznym (t.j. "czysta"). Nawet operacje typu IO (Input/Output) sa niczym objasnienia tego, co nalezy zrobic, stworzone przez czysty kod. Nie ma 'statements' czy 'instructions', a sa jedynie 'expressions', ktore nie moga mutowac zmiennych (lokalnych i globalnych) i nie maja dostepu do czasu czy losowych numerow.

OK, wiec czemu w ogole powinnismy byc zainteresowani FP w JS? Ufam, ze zainteresowanie FP w JS zaczelo wzrastac wraz z popularnoscia:

1. ES6+, ktore dodalo ogrom pomyslow z FP i generalnie popchnelo JS w strone deklaratywnosci, a nie imperatywnosci.
1. React, ktore przemycilo calkiem sporo konceptow FP do "mainstreamu".

React zajmiemy sie dopiero pozniej. Najpierw musimy poznac i lepiej zrozumiec, o co w ogole chodzi w funkcyjnym programowaniu.

Milego warsztatu!

## Ramda docs - jak czytac anotacje

Zanim przejdziemy do konceptow FP w JS, to rzucmy okiem na dokumentacje [Ramdy](https://ramdajs.com/docs/#drop), a konkretnie na metode `R.drop`. Zaraz pod nazwa metody, znajdziemy tzw. annotacje (annotation), ktora wyjasnia uzycie metody. `R.drop` dziala dla kolekcji i dla stringow, wiec dostajemy dwie annotacje. Wygladaja nastepujaco:

- kolekcja: `Number → [a] → [a]`
- string: `Number → String → String`

Czytamy anotacje nastepujaco:

1. `Number ->` - pierwszy parametr metody (input)
1. `[a] ->` - drugi parametr metody (input)
1. `[a]` - wartosc wyjsciowa (output)

`a` znaczy tyle, co "any", wiec moze to byc jakikolwiek typ danych. Funkcja strzalkowa, wobec tego, moglibysmy sobie wyobrazic implementacje `R.drop` w uproszczeniu w taki sposob: `(number, arrayOfAny) => newArrayOfAny`.

Uzycie:

```javascript
R.drop(1, [1, 2, 3]); // [2,3]
```

Dajemy numer (`Number`), dajemy kolekcje (`[a]`), otrzymujemy nowa kolekcje (`[a]`) pomniejszona o `Number` elementow z kolekcji.

Tak samo ma sie sprawa z wersja dla stringow.

1. `Number ->` - pierwszy parametr metody (input)
1. `String ->` - drugi parametr metody (input)
1. `String` - wartosc wyjsciowa (output)

Uzycie:

```javascript
R.drop(3, "nieoświecony"); // "oświecony"
```

Na poczatku, czytanie anotacji moze byc trudne, ale z czasem pozwala na szybkie zrozumienie dzialania metody, zwlaszcza tam, gdzie parametr oczekuje funkcji.

Tutaj prosty przyklad z uzyciem [`R.times`](https://ramdajs.com/docs/#times), ktorego anotacja jest taka: `(Number → a) → Number → [a]`. Tutaj pierwszy parametr przyjmuje funkcje: `(Number → a)`, ktora nalezy rozumiec tak:

1. `Number ->` - pierwszy parametr (input)
1. `a` - wartosc wyjsciowa o dowolnym typie (output)

Zauwazcie, ze w anotacji mamy relacje pomiedzy `a`, ktore wychodzi z funkcji w pierwszym parametrze, a `[a]`, ktore wychodzi ostatecznie z metody `R.times`. To nie przypadek. Typ danych na wyjsciu z funkcji callbackowej bedzie typem danych, ktore wyjda z `R.times` (tyle, ze wewnatrz kolekcji).

Uzycie:

```javascript
R.times(() => "hi", 5); // ["hi", "hi", "hi", "hi", "hi"]
R.times(() => 1, 3); // [1, 1, 1]
R.times((n) => n * n, 10); // [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Czytanie anotacji rozwija "funkcyjne" myslenie :-)

# Koncepty funkcyjnego programowania uzywane w JS

Mala notka na wstepie. Poniewaz uzywamy funkcyjnego paradygmatu, keyword `this` nie interesuje nas w najmniejszym stopniu, wobec czego mozemy uzywac fat arrow function (`=>`) ile wlezie.

## Pure functions

Spelnione musza byc dwa warunki, zebysmy mogli otrzymac funkcje typu pure ("czysta"; w odroznieniu od impure).

1. Niezaleznie od tego, ile razy wywolamy funkcje, zawsze otrzymamy ten sam rezultat (output) przy tych samych
   argumentach (input).
2. Funkcja nie stworzy zadnych efektow ubocznych (side-effects), np. nie wywola HTTP request, ktory bedzie
   mial wplyw na inny system (serwer, baze danych, etc.).

Przyklady:

```javascript
// funkcja otrzymuje jakis numer jako argument (n) i dodaje do niego 1
const addOne = (n) => n + 1;

// wielokrotne wywolanie funkcji z tym samym argumentem (input) produkuje ten sam rezultat (output)
addOne(2); // 3
addOne(2); // 3
addOne(2); // 3
```

Kiedy zaczyna sie niebezpieczenstwo impure functions? Wtedy, gdy nasza funkcja moze zaczac produkowac rozne rezultaty.

```javascript
let someNum = 1;

const addOne = (n) => n + someNum;

addOne(2); // 3
addOne(2); // 3
addOne(2); // 3
someNum++;
addOne(2); // 4
addOne(2); // 4
addOne(2); // 4
```

Albo gorzej!

```javascript
let someNum = 1;

const addOne = (n) => {
  someNum++;
  return n + someNum;
};

addOne(2); // 4
addOne(2); // 5
addOne(2); // 6
```

Czy kazde uzycie jakiejs wartosci spoza funkcji sprawia, ze funkcja staje sie impure? Nie koniecznie.

```javascript
const someNum = () => 1;

const addOne = (n) => n + someNum();

addOne(2); // 3
addOne(2); // 3
addOne(2); // 3
```

Teraz `someNum` tez jest czysta funkcja, wobec czego wyprodukowana wartosc nigdy sie nie zmieni! Pure awesomeness 😀

Idac dalej, pure functions zaczynaja byc ciekawsze, gdy bawimy sie kolekcjami.

```javascript
const numbers = [1, 2, 3];

const setArr = (arr, n) => [...arr, n]; // tworzymy nowy array z przekazanego array i liczba n

setArr(numbers, 4); // [1,2,3,4]
setArr(numbers, 4); // [1,2,3,4]
setArr(numbers, 4); // [1,2,3,4]
// ^^ zawsze ten sam rezultat, bo nie mutujemy array numbers, na ktorym dzialamy!

const newNumbers = setArr(numbers, 4); // [1,2,3,4]
// ^^ zdecydowanie chcemy dzialac w ten sposob (stary array jest nienaruszony, a nowy jest kopia starego + nowym elementem)
```

Podobnie uzywamy pure functions w funkcjach wyzszego rzedu typu `.map`, gdzie przekazana funkcja staje sie funkcja callbackowa.

```javascript
const numbers = [1, 2, 3];

const newNumbers = numbers.map((n) => n + 1); // [2,3,4]
// ^^ tutaj anonimowa funkcjs strzalkowa jest czysta
```

Czyste funkcje daja nam wieksza pewnosc i przyczyniaja sie do latwiejszych do ogarniecia testow. Z punktu widzenia FP, powinnismy ich uzywac zawsze, jesli to tylko mozliwe. Czy zawsze bedzie? Nie, tam gdzie dzialamy z Web API musimy porzucic je na rzecz impure functions.

## Immutability

Jedna z najprzyjemniejszych cech funkcyjnego jezyka jest niezmiennosc danych (immutability). Niestety, wiekszosc danych w JS jest mutowalna (z wyjatkiem nastepujacych prymitywnych: string, number, bool, null, i undefined). Na szczescie, Ramda library kopiuje wszystkie dane, a wiec transformuje dane zamiast je mutowac.

Prosty przyklad:

```javascript
const kidToys = ["ball", "dinosaur", "lollypop"];

const stuff = R.map((toy) => ({ name: toy, used: true }), kidToys); // [{name: 'ball', used: true}, ...]

const kidToys2 = R.append("buzz astral", kidToys); // ['ball', 'dinosaur', 'lollypop', 'buzz astral']
```

`stuff` jest nowym array (po transformacji), ktory w zaden sposob nie wplywa na ksztalt `kidToys` array.
`kidToys2` jest nowym array (Ramda robi to za nas).

Vanilla JS:

```javascript
const kidToys = ["ball", "dinosaur", "lollypop"];

const stuff = kidToys.map((toy) => ({ name: toy, used: true })); // [{name: 'ball', used: true}, ...]

const kidToys2 = [...kidToys, "buzz astral"]; // ['ball', 'dinosaur', 'lollypop', 'buzz astral']
```

Ramda, z natury swego API (ktore jest lustrzanym odbiciem tego, co znajdziemy w czystych jezykach funkcyjnych), zawsze dba o niemutowalnosc danych dla nas. Podczas, gdy powyzsze przyklady (`R.map`, `R.append`) sa latwe do zrobienia w vanilla JS, to Ramda zaczyna lsnic przy dalszej eksploracji metod biblioteki.

```javascript
const kidToys = ["ball", "dinosaur", "lollypop"];

const lessToys = R.drop(2, kidToys); // ['lollypop']
const oneToy = R.takeLast(1, kidToys); // ['lollypop']
```

Za kazdym razem dostajemy nowa kolekcje, bez mutowania starej. Dodatkowym milusinskim plusem jest to, ze dostajemy dostep do duzej ilosci metod, ktore usprawniaja nasz workflow.

## First-class functions

Funkcje w JS, podobnie jak w jezykach funkcyjnych, sa funkcjami pierwszej kategorii (czasem nazywanymi tez obywatelami pierwszej kategorii (first-class citizens), tzn. funkcje moga brac inne funkcje jako argumenty (input), oraz zwracac funkcje (output).

```javascript
const addOne = (n) => n + 1;

// Ramda
R.map(addOne, [1, 2, 3]); // [2,3,4]

// Vanilla
[1, 2, 3].map(addOne); // [2,3,4]
```

```javascript
const returnFn = () => () => 1;

// Ramda
R.map(addOne, [1, 2, 3]); // [2,3,4]

// Vanilla
returnFn(); // () => 1
returnFn()(); // 1
```

## Higher-order functions

Funkcje wyzszego rzedu to funkcje, ktore przyjmuja funkcje callbackowa w parametrze lub/i zwraca funkcje. (Kilka przykladow na [wiki](https://en.wikipedia.org/wiki/Higher-order_function#JavaScript))

Mozemy wiec miec funkcje przyjmujace funkcje oraz dane, gdzie funkcja zadziala na danych i zwroci nowe dane:

```javascript
const applyFnToNum = (fn, n) => fn(n);

applyFnToNum((n) => n * n, 8); // 64
```

Lub zwrocic funkcje:

```javascript
const applyFnToNumAndReturnFn = (fn) => (n) => fn(n);

applyFnToNumAndReturnFn((n) => n * n); // n => fn(n)
applyFnToNumAndReturnFn((n) => n * n)(8); // 64

// czesto robimy to "krokami", w ten sposob:
const timesNum = applyFnToNumAndReturnFn((n) => n * n);
timesNum(8); // 64
```

Typowymi funkcjami wyzszego rzedu, bez ktorych zaden jezyk funkcyjny obyc sie nie moze, sa `map`, `filter`, oraz `reduce`.

`map` pozwala nam transformowac dane w kolekcji.  
`filter` pozwala nam wybierac konkretne dane z kolekcji.  
`reduce` pozwala nam zredukowac dane z kolekcji.

Przyklady:

```javascript
// map
// Ramda
R.map((x) => x + x, [1, 2, 3]); // [2, 4, 6]
// Vanilla
[1, 2, 3].map((x) => x + x); // [2,4,6]

// filter
// Ramda
R.filter((x) => x > 2, [1, 2, 3]); // [3]
// Vanilla
[1, 2, 3].filter((x) => x > 2); // [3]

// reduce
// Ramda
R.reduce((acc, x) => acc + x, 0, [1, 2, 3]); // (1+2+3) == 6
// Vanilla
[1, 2, 3].reduce((acc, x) => acc + x, 0); // 6
```

Oczywiscie, mozliwosci tych funkcji sa ogromne. W dodatku, czesto uzywamy function chaining, zeby uzywac ich razem.

```javascript
const players = [
  { id: 1, name: "Tom", score: 68 },
  { id: 2, name: "Sara", score: 72 },
  { id: 3, name: "Peter", score: 46 },
];

// Vanilla
players
  .filter(({ score }) => score >= 50)
  .reduce((acc, player) => ({ passed: acc.passed + 1 }), { passed: 0 }); // {passed: 2}

// Ramda
const getPassedPlayers = R.compose(
  R.reduce((acc, player) => ({ passed: acc.passed + 1 }), { passed: 0 }),
  R.filter(({ score }) => R.gte(score, 50))
);

getPassedPlayers(players); // {passed: 2}
```

Zauwaz kilka rzeczy:

1. Ramda uzywa specjalnej funkcji (`R.compose`), zeby skomponowac funkcje w jeden "chain".
1. Dodatkowo, bardziej organicznym zapisem jest przypisac cala kompozycje do `const`.
1. W koncu, zwroc uwage na uzycie dodatkowej funkcji `R.gte`. Ona nie jest wymagana. Moglibysmy uzyc `score >= 50`, ale `R.gte` wiedzie nas glebiej w arkana czysto funkcyjnego programowania :)

Ta mala wycieczka w strone wiekszej funkcyjnosci poprzez `R.gte` jest dobrym krokiem w strone "oswiecenia": w funkcyjnym programowaniu, "wszystko" jest funkcja 😌

## Declarative code

Piszac JS w paradygmacie FP bedziemy (zupelnie, jak w jezykach funkcyjnych) uzywac deklaratywnego, raczej niz imperatywnego kodu. Jaka jest roznica pomiedzy stylem imperatywnym a deklaratywnym (imperative vs declarative)? Najprosciej rzecz ujmujac, imperatywny styl zmusza programiste do pisania w stylu JAK (HOW). Natomiast deklaratywny styl pozwala na pisanie w stylu CO (WHAT). A wiec nasze myslenie nie moze juz byc "jak mam to zrobic?", ale "co mam zrobic?" (gdyby komputer nas pytal 😉).

Wysmienitym przykladem jest uzywanie funkcji `R.map` (ktora jest podobna do natywnej metody `Array.prototype.map`), ktora jest deklaratywna alternatywa do petli typu `for` (for loop), i wyglada nastepujaco:

```javascript
// Ramda
R.map((n) => n * n, [1, 2, 3]); // [1,4,9]

// Vanilla
[1, 2, 3].map((n) => n * n); // [1,4,9]
```

`R.map` bierze liste liczb (`[1,2,3]`), i aplikuje anonimowa funkcje (`=>`) do kazdej z nich. Dla celow wylacznie edukacyjnych, tu jest kod, ktory dokona tego samego, ale z uzyciem nazwanej funkcji w miejscu anonimowej):

```javascript
const dblNumber = (n) => n * n[(dblNumber(1), dblNumber(2), dblNumber(3))]; // [1,4,9]
```

Nie interesuje nas JAK `R.map` zmienia kazdy element podanej listy. Obchodzi nas jedynie, CO ma zostac zrobione.

Gdybysmy pisali to w stylu imperatywnym, musielibysmy zrobic to pewnie tak:

```javascript
const arr = [1, 2, 3];
const arr2 = [];
const dblNumber = (n) => n * n;

for (let i = 0; i < arr.length; i++) {
  arr2.push(dblNumber(arr[i]));
}

arr2; // [1,4,9]
```

Deklaratywny styl pozwala na pisanie mniej kodu, ktory w dodatku jest bardziej przejrzysty. Refaktoring tez staje sie latwiejszy.

## Currying

Prawdopodobnie jeden z najbardziej swoistych terminow w FP :) Nie, nie chodzi o smarowanie czegos z uzyciem sosu curry! Termin pochodzi od konkretnego gentlemena: [Haskell Curry](https://en.wikipedia.org/wiki/Haskell_Curry).

Z definicji, rozwijanie funkcji (lub 'currying') to proces konwertowania funkcji, ktora bierze wiele argumentow w sekwencje funkcji, gdzie kazda funkcja bierze tylko jeden argument.

[Currying na wiki](https://en.wikipedia.org/wiki/Currying)

Najlepiej pokaze to prosty przyklad:

```javascript
// Zamiast napisac funkcje add w ten sposob
const add = (a, b) => a + b;

// Napiszemy ja tak
const add = (a) => (b) => a + b;

// I wywolamy ja nastepujaco
const sum = add(1)(2); // 3

// Nie musimy podawac jej wszystkich argumentow od razu, mozemy robic to stopniowo
const addOne = add(1);
addOne(2); // 3

// Uzywajac funkcji z Ramdy, zrobmy to tak
const addOne = R.add(1);
addOne(2); // 3

// Mala dygresja: jesli chcesz dodac 1, mozesz tez uzyc R.inc
R.inc(2); // 3
```

Uzywajac troche wiekszego przykladu, Ramda pozwala nam na bardzo latwy "currying":

```javascript
const timesThree = (a, b, c) => a * b * c;
const curriedTimesThree = R.curry(timesThree);
const timesTwoFn = curriedTimesThree(2, 4); // 8
timesTwoFn(3); // 24
```

## Popularne operacje na kolekcjach

Jezyki funkcyjne typowo maja w swoich podstawowych bibliotekach zbior funkcji do pracy na kolekcjach. Ramda.js nie jest inna.

```javascript
// head
// bierze pierwszy element z listy lub string
R.head(["fi", "fo", "fum"]); //=> 'fi'
R.head("abc"); //=> 'a'

// tail
// odwrotnosc head. Zwraca cala liste lub string z wyjatkiem pierwszego elementu
R.tail([1, 2, 3]); //=> [2, 3]
R.tail("abc"); //=> 'bc'

// zip
// laczy ze soba dwie listy w krotki (tuples). JS nie ma krotek, wiec otrzymujemy array of arrays.
R.zip([1, 2, 3], ["a", "b", "c"]); //=> [[1, 'a'], [2, 'b'], [3, 'c']]

// take
// bierze liczbe elementow z listy zaczynajac od lewej
R.take(2, ["foo", "bar", "baz"]); //=> ['foo', 'bar']

// drop
// przeciwienstwo 'take'. Odrzuca liczbe elementow z listy zaczynajac od lewej
R.drop(2, ["foo", "bar", "baz"]); //=> ['baz']

// all
// sprawdza, czy wszystkie elementy listy zgadzaja sie z predykatem
R.all(R.equals(3))([3, 3, 1, 3]); //=> false

// any
// sprawdza, czy ktorykolwiek z elementow listy zgadza sie z predykatem
R.any((n) => n < 0)([1, 2]); //=> false
R.any(R.flip(R.lt)(0))([1, 2]); //=> false
R.any(R.lt(0))([1, 2]); //=> true

// includes
// sprawdza, czy element jest w liscie/string
R.includes(3, [1, 2, 3]); //=> true
R.includes(4, [1, 2, 3]); //=> false
R.includes({ name: "Fred" }, [{ name: "Fred" }]); //=> true
R.includes([42], [[42]]); //=> true
R.includes("ba", "banana"); //=>true
```

## Pipe oraz Compose

Funkcyjne jezyki czesto pozwalaja na 'pipe'owanie' oraz na 'function composition'. To pierwsze we [front-endowym jezyku Elm](http://elm-lang.org/) wyglada tak:

```elm
100 |> (+) 2 |> (*) 10 |> negate   -- -1020
```

Natomiast kompozycja funkcji tak:

```elm
fn = (+) 2 >> (*) 10 >> negate
fn 100  -- -1020
```

Jako, ze JS nie ma takich operatorow, Ramda przychodzi nam z pomoca oferujac dwie funkcje: `R.pipe` i `R.compose`. Uzywamy ich nastepujaco:

```javascript
R.pipe(R.add(2), R.multiply(10), R.negate)(100); // -1020

// oraz

R.compose(R.negate, R.multiply(10), R.add(2))(100); // -1020
```

`pipe` pozwala nam przeslac dane kolejno przez wybrane funkcje, natomiast `compose` komponuje funkcje razem ('laczy' je), i pozwala przeslac do nich dane w formie argumentu, jak gdyby do jednej funkcji.

Zauwaz, ze w innej kolejnosci ukladamy argumenty w `R.pipe` i w `R.compose`. Na poczatku warto opanowac jedna z tych metod :)

Zobaczmy troche prostszy przyklad:

```javascript
// compose
const incrByThree = R.compose(R.inc, R.add(2));
incrByThree(100); // 103

// pipe
const incrByThreeWithPipe = R.pipe(R.add(2), R.inc);
incrByThreeWithPipe(100); // 103
```

# Przykladowe uzycia

## Wybieranie z listy

Wybieranie z kolekcji to kwestia uzycia `find` lub `filter`.

```javascript
const users = [
  { id: 1, name: "A" },
  { id: 2, name: "B" },
  { id: 3, name: "A" },
];

// Ramda
R.find(R.propEq("name", "A"))(users); // znajduje jedynie {id: 1, name: "A"}
R.filter(R.propEq("name", "A"))(users); // znajduje wszystkich userow spelniajacych predykat: [{id: 1, name: "A"}, {id: 3, name: "C"}]

// Vanilla
users.find((user) => user.name === "A"); // znajduje jedynie {id: 1, name: "A"}
users.filter((user) => user.name === "A"); // znajduje wszystkich userow spelniajacych predykat: [{id: 1, name: "A"}, {id: 3, name: "A"}]
```

## Dodawanie do Listy

Dodawanie elementu do listy zawsze stworzy nowa liste.

Dodajmy element do listy:

```javascript
const arr = ["write", "more"];
// Ramda
R.append("tests", arr); //=> ['write', 'more', 'tests']

// Vanilla
[...arr, "tests"];
```

Mozemy tez polaczyc dwie listy lub strings:

```javascript
const arr1 = [4, 5, 6];
const arr2 = [1, 2, 3];

// Ramda
R.concat(arr2, arr1); //=> [4, 5, 6, 1, 2, 3]
R.concat("ABC", "DEF"); // 'ABCDEF'

// Vanilla
[...arr2, ...arr1];
`${"ABC"}${"DEF"}`;
```

## Usuwanie z Listy

Usuwanie z listy wymaga uzycia funkcji z modulu `R.filter`. Filter pozwala nam 'filtrowac' elementy w liscie, i wybrac jedynie te, o ktore nam chodzi.  
Filtrowanie odbywa sie za pomoca anonimowej funkcji (lub nazwanej), ktora przyjmuje jeden argument (element z listy), a produkuje `boolean`.
Funkcja bedzie wywolana dla kazdego elementu listy.

W ponizszym przykladzie uzywamy operatora `(!==)`, a wiec 'nie rowna sie', i 'usuwamy' z listy liczbe `1`.

```javascript
// Ramda
R.filter((n) => n !== 1, [1, 2, 3]); // [2,3]

// lub uzywaja R.pipe

R.filter(R.pipe(R.equals(1), R.not), [1, 2, 3]); // [2,3]

// Vanilla
[1, 2, 3].filter((n) => n !== 1);
```

Mozemy smialo usunac z listy wiecej niz jeden element.

W przykladzie ponizej wybieramy jedynie liczby, ktore rownaja sie 3 lub sa wieksze (operator `(>=)`).

```javascript
// Ramda
R.filter(R.flip(R.gte)(3), [1, 2, 3, 4, 5]); // [3,4,5]

// Vanilla
[1, 2, 3, 4, 5].filter((n) => n >= 3);
```

## Update'owanie Listy

Czasem musimy zmienic konkretny element (lub elementy) w liscie lub zmienic nawet wszystkie elementy. W funkcyjnych jezykach do transformacji list sluzy nam funkcja `map`.

Przyklad update'u wszystkich elementow listy:

```javascript
const people = [
  { name: "Tom", age: 22 },
  { name: "Dick", age: 33 },
  { name: "Mortimer", age: 44 },
];

// Ramda
R.map((n) => R.add(n, n), [1, 2, 3]); // [2,4,6]

R.map((person) => ({ ...person, age: R.inc(person.age) }), people);
// [{name: "Tom", age: 23}, {name: "Dick", age: 34}, {name: "Mortimer", age: 45}]

// Vanilla
people.map((person) => ({ ...person, age: person.age + 1 }));
```

Przyklad zmiany jedynie osoby `Tom`.

```javascript
const people = [
  { name: "Tom", age: 22 },
  { name: "Dick", age: 33 },
  { name: "Mortimer", age: 44 },
];

//Ramda
R.map(
  (person) =>
    person.name === "Tom" ? { ...person, age: R.inc(person.age) } : person,
  people
);
// [{name: "Tom", age: 23}, {name: "Dick", age: 33}, {name: "Mortimer", age: 44}]

// Vanilla
people.map((person) =>
  person.name === "Tom" ? { ...person, age: R.inc(person.age) } : person
);
```

## Wybieranie z Listy

Wybieranie z listy jest podobne do usuwania z listy. Tu rowniez uzywamy funkcji `R.filter`. Rozbilem usuwanie i wybieranie to na dwa koncepty, bo tak dyktuje mi doswiadczenie w uczeniu FP.

```javascript
const names = ["Mary", "Yousef", "John", "Mark"];

// Ramda
R.filter((str) => str === "John", names); // ["John"]

// Vanilla
names.filter((str) => str === "John"); // ["John"]
```

Notka: Zauwaz prosze, ze wybierajac cos z listy, otrzymujemy nie sam element, ale liste z elementem (lub wieloma elementami). Wiec jesli chcemy wybrac, to musimy tak:

```javascript
const names = ["Mary", "Yousef", "John", "Mark"];

// Ramda
R.compose(
  R.head,
  R.filter((str) => str === "John")
)(names); // "John"

// Vanilla
names.filter((str) => str === "John")[0]; // "John"
// mozna tez uzyc names.find()
```

# Cwiczenia

Do cwiczen potrzebujesz biblioteki Ramda.js. Polecam uzyc

- [REPL](https://ramdajs.com/repl?v=0.27.0), lub
- [zainstalowac](https://ramdajs.com/) Ramde lokalnie

Mozesz tez uzyc Vanilla JS.

```javascript
// Dane:
// =====
const PEOPLE = [
  { name: "John", age: 10 },
  { name: "Jack", age: 20 },
  { name: "Jake", age: 25 },
  { name: "Jesse", age: 39 },
  { name: "Jill", age: 54 },
];

// Cwiczenie 1:
// ============
// Uzyj R.compose() do uproszczenia ponizszej funkcji.
// https://ramdajs.com/docs/#compose
const theirName = function (people) {
  const the_last_person = R.last(people); // R.last wyciaga ostatni element z array
  return R.prop("name", the_last_person); // R.prop wyciaga wartosc po kluczu
};

// Cwiczenie 2:
// ============
// Uzyj R.pipe() oraz R.sum, R.add, R.inc, R.negate, zeby
// - zsumowac array [2,3]
// - dodac do niego 3
// - zwiekszyc o jeden
// - zanegowac
//
// Poprawny wynik: -9
// https://ramdajs.com/docs/#pipe
// https://ramdajs.com/docs/#sum
// https://ramdajs.com/docs/#add
// https://ramdajs.com/docs/#inc
// https://ramdajs.com/docs/#negate
const newNumber = undefined;

// Cwiczenie 3:
// ============
// Uzyj R.compose(), R.prop(), oraz R.head(), zeby zwrocic imie pierwszej osoby.
// https://ramdajs.com/docs/#compose
// https://ramdajs.com/docs/#prop
// https://ramdajs.com/docs/#head
const nameOfTheFirstPerson = undefined;

// Cwiczenie 4:
// ============
// Uzyj R.compose(), R.map(), R.inc(), oraz R.filter(), zeby postarzec kazda z osob o 1 rok,
// i zwrocic tylko tych, ktorzy maja 25 lat lub mniej.
// https://ramdajs.com/docs/#compose
// https://ramdajs.com/docs/#map
// https://ramdajs.com/docs/#inc
// https://ramdajs.com/docs/#filter
const onlyTwentyFiveYearOlds = undefined;

// Cwiczenie 5:
// ============
// Uzyj R.map() do stworzenia nowego array na bazie PEOPLE array, ktorego kazdy
// element bedzie wygladal tak: [name, age] (np. ["John", 10]).
// https://ramdajs.com/docs/#map
const newPeople = undefined;

// Cwiczenie 6:
// ============
// Uzyj R.pipe, R.innerJoin(), i R.reduce() oraz R.add() do stworzenia nowego array
// na bazie PEOPLE array.
// 1. R.innerJoin() powinien zbudowac nowy array jedynie z osob o wieku 10, 25, i 54.
// 2. Uzyj R.reduce() i R.add(), zeby zsumowac wiek ludzi.
//
// Poprawny wynik: 89
// https://ramdajs.com/docs/#pipe
// https://ramdajs.com/docs/#innerJoin
// https://ramdajs.com/docs/#reduce
// https://ramdajs.com/docs/#add
const summedUpAges = undefined;

// Cwiczenie 7:
// ============
// Powtorz poprzednie cwiczenie, z jednym wyjatkiem. Zamiast uzywac R.reduce() i R.add(),
// uzyj R.sum() do zsumowania.
//
// Poprawny wynik: 89
// https://ramdajs.com/docs/#pipe
// https://ramdajs.com/docs/#innerJoin
// https://ramdajs.com/docs/#sum
const summedUpAges2 = undefined;

// Cwiczenie 8: relaksujace :)
// ============
// Przeksztalc i porownaj arrays :-)
// Data:
const NUMS = [1, 2, 3, 4, 3, 2, 1];
let arr1;
let arr2;
// - nadpisz arr1 uzywajac R.take tak, zeby uzyskac [1,2,3]
// - nadpisz arr2 uzywajac R.takeLast oraz R.reverse tak, zeby uzyskac [1,2,3]
// - porownaj obydwa arrays uzywajac R.equals (powinny byc sobie rowne)
```

# Przyklady uzycia funkcyjnego paradygmatu

Reasumujac, JavaScript jest bardzo luznym (w sensie typowania) jezykiem (w przeciwienstwe do statycznych jezykow jak Elm czy Haskell), ktory nie zmusza nas do pisania funkcji czy programow w specyficzny sposob. To jest... niekoniecznie swietne ;-) Dzis, z pomoca przychodzi nam programowanie funkcyjne. A zatem, FP zacheca nas do trzymania sie nastepujacych zasad:

- trzymac sie z dala od mutacji
- trzymac sie z dala od efektow ubocznych
- trzymac sie z dala od nieczystych funkcji (gdzie to mozliwe)

Lekarstwem na powyzsze problemy jest trzymanie sie nastepujacych zasad:

- piszmy czyste funkcje
- piszmy deklaratywny kod
- piszmy male funkcje, z ktorych mozemy komponowac wieksze funkcje, z ktorych z kolei bedzie skomponowany nasz program

## React

### Nie dla mutacji

W JS mozemy uniknac mutacji obiektow literalnych oraz arrays uzywajac spread operator (`...`) (o czym za chwile). Mozemy tez uniknac mutacji piszac proste czyste funkcje, ktore wyrazaja komponenty naszego UI. React jest o tyle zabawny, ze historycznie zaczal od "OOP" (komponenty klasowe), pozniej mieszkal OOP z FP (komponenty funkcyjne), a teraz zdaje sie plynac na fali FP (Hooks, Redux/Context, etc.). Tak naprawde, sam React uzywa sporo funkcyjnych pomyslow, i zeby naprawde popchnac o stopien wyzej, chcielibysmy zaczac korzystac z biblioteki typu Flow oraz Immutable.JS albo zainwestowac w TypeScript. Te dwie przyjemnosci nie sa w granicach tego warsztatu. Ale na pewno sa warte rozwazenia.

```javascript
const Welcome = ({ name }) => <h1>Hello, {name}.</h1>;
```

Czysta funkcja bez zadnych efektow ubocznych i mutacji. Latwa w zrozumieniu oraz testowaniu (unit testing), w ktorej trudno o bugi. Niezaleznie od ilosci wywolan, dla tego samego inputu zawsze otrzymamy ten sam output.

Pomimo tego, ze spread operator nie jest znany w jezykach funkcyjnych, to w JS (by uniknac mutowania kolekcji i rekordow), chcemy po niego siegac i jest (jak gdyby) czescia funkcyjnego paradygmatu.

```javascript
const nums = [1,2,3,4,5,6]

// Vanilla
[...nums].reverse()[0]   // => 6

// uzywajac Ramdy, mozemy to napisac w duzo bardziej deklaratywny sposob
// ktory jest odbiciem tego, co zrobilibysmy w jezyku funkcyjnym
R.pipe(R.reverse, R.head)(nums)  // => 6

// dla urzeczywistnienia podobienstwa, tutaj jest ta sama operacja, ale w Elm
// (uzywajac pipe operator (|>))
nums |> List.reverse |> List.head // Just 6
// tutaj kolekcja nie ulega mutacji poniewaz mutacja w Elm nie jest w ogole mozliwa
```

Niemniej jednak, bez Ramdy, spread staje sie jednym z podstawowych narzedzi funkcyjnego paradygmatu w JS.

### Tak dla deklaratywnego kodu

```javascript
const nums = [1, 2, 3, 4, 5, 6];

// imperatywnie
const newNums = [];
for (let i = 0; i < nums.length; i++) {
  newNums.push(ns[i]++); // mutacja newNums !!
}
newNums; // [2, 3, 4, 5, 6, 7]

// deklaratywnie
// Vanilla
const newNums = nums.map((n) => n + 1); // [2, 3, 4, 5, 6, 7]

// Ramda
const newNums = R.map(R.inc, nums); // [2, 3, 4, 5, 6, 7]
// to samo, co R.map(n => R.inc(n), nums)
```

W React swietnym przykladem deklaratywnej natury frameworku jest funkcja `render` z `ReactDom`, ktora (gdyby pisana na wzor imperatywny) bylaby raczej trudna.

```jsx
const { render } = ReactDom;

const Welcome = () => (
  <div id="welcome">
    <h2>Hello!</h2>
  </div>
);

render(<Welcome />, document.getElementById("app"));
```

Deklaratywnosc zawsze polega na zwiezlosci i wyrazniu CO powinno sie wydarzyc, a nie JAK. Nie obchodzi nas, jak React renderuje sobie swoje UI, po prostu mowimy mu co ma zrobic.

## Tak dla kompozycji z malych funkcji

W React typowym jest czeste uzycie funkcji wyzszego rzedu:

- `map`
- `filter`
- `reduce`

Ich uzycie nie rozni sie w zasadzie od uzycia, ktore juz poznalismy, bo w JSX piszemy po prostu JavaScript. To, zreszta, umozliwia nam latwo uzywac Ramdy (`npm i -S ramda`), a potem:

```jsx
import * as R from "ramda";

const ComponentA = ({ arr }) => {
  const isNotEmpty = (arr) => R.not(R.isEmpty(arr));

  return (
    <>
      {isNotEmpty(arr) && (
        <ul>
          {R.take(3, arr).map((c) => (
            <ComponentB key={c.id} {...c} />
          ))}
        </ul>
      )}
    </>
  );
};
```

W przykladzie powyzej, zauwazcie, ze wymieszalem uzycie funkcji Ramdowej `R.take` bezposrednio z function chaining JSowym (`.map`), bo to totalnie dziala :-) Alternatywa jest oczywiscie uzyc `R.pipe` lub `R.compose`. Mamy, zatem, sporo manewru w tym, jak daleko chcemy sie posunac z uzywaniem Ramdy lub innej biblioteki (np. [lodash](https://lodash.com/)). Bardzo przyjemnie.

Swietnym przykladem jest tez kompozycja komponentow, gdzie jeden funkcyjny komponent zwraca inny, a moze tez smialo brac jeszcze inny komponent jako argument. A wiec funkcja przyjmuje funkcje.

```jsx
const Welcome = ({ name }) => {
  return <h1>Hello, {name}</h1>;
};

const App = () => {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
};

// App zwraca trzy Welcome komponenty
ReactDOM.render(<App />, document.getElementById("root"));
```

A tu prosty przyklad komponentu, ktory owija inny, wobec czego jest komponentem wyzszego rzedu, bo bierze inny komponent jako parametr.

```jsx
const withTitle = (title) => (Component) => (props) =>
  (
    <div>
      <h1>{title}</h1>
      <Component {...props} />
    </div>
  );

const MyComponentWithTitle = withTitle("Hi, Mom!")(MyComponent);
```

Jak widzicie, znajac juz podstawy FP, mozecie zaczac aplikowac je do

# Cwiczenia 2

Do wyboru:

- Zbuduj malutka appke z React uzywajac Ramdy. Uzyj [Create React App](https://create-react-app.dev/) do kickstartniecia appki..
- Zbuduj malutka appke uzywajac jedynie Ramdy i Vanilla JS.
- Jezeli masz cos zbudowanego albo nad czyms pracujesz, i chcialbys zobaczyc, jak mozesz wykorzystac swoja swiezo-nabyta wiedze do przerobienia kawalku kodu czy komponentu w React, to smialo!

A jesli czujesz sie naprawde odwaznie, zachecam do sprawdzenia typowego przykladu [Counter App](https://ellie-app.com/new) napisanej w [Elm](https://elm-lang.org): czysto funkcyjnym jezyku, ktory kompiluje sie do JS, a w ktorym _fizycznie_ nie da sie napisac innej, anizeli czystej funkcji! (Choc side-effects sa jak najbardziej mozliwe, ale "kontrolowane" przez Elm (i jest to znaczaca latwiejsze do ogarniecia, anizeli w Haskell (zapomnij o monadach ;)))). Link do Counter App pozwoli Ci pisac, zapisywac, i debugowac (bardzo mily time-travel debugger mozliwy dzieki niemutowalnym stanom aplikacji) kod w Elm.

Tutaj szybki wstep typu JS-to-Elm: [From Javascript?](https://elm-lang.org/docs/from-javascript)

# Uzyteczne linki

## JS biblioteki

- [RamdaJS](https://ramdajs.com/)
- [lodash](https://lodash.com/)
- [Flow](https://flow.org)
- [Immutable](https://immutable-js.github.io/immutable-js/)
- [Chai Immutable](https://github.com/astorije/chai-immutable)

## Frontendowe jezyki funkcyjne kompilujace sie do JS

- [Elm](https://elm-lang.org)
- [Purescript](https://www.purescript.org/)
- [Clojurescript](https://clojurescript.org/)

## Artykuly

- [Why Ramda?](https://fr.umio.us/why-ramda/)
- [Why curry helps?](https://hughfdjackson.com/javascript/why-curry-helps/)
- [Why OO Sucks by Joe Armstrong (tworca Erlang)](http://harmful.cat-v.org/software/OO_programming/why_oo_sucks)

## Materialy do dalszej zglebiania arkan FP 😌

- [Awesome FP JS](https://github.com/stoeffel/awesome-fp-js)
- [Learning Functional Programming with JavaScript - Anjana Vakil - JSUnconf](https://www.youtube.com/watch?v=e-5obm1G_FY)
- [6 best JavaScript Functional programming libraries](https://www.dunebook.com/best-javascript-functional-programming-libraries/)

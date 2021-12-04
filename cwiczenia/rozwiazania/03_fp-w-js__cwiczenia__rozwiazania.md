# Funkcyjne programowanie w JavaScript

Cwiczenia. Rozwiazania.

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
const theirName = (people) => R.compose(R.prop("name"), R.last)(people);
// lub
const theirName = (people) => {
  let composedFn = R.compose(R.prop("name"), R.last);
  return composedFn(people);
};
theirName(PEOPLE); // => 'Jill'

// Cwiczenie 2:
// ============
// Uzyj R.pipe() oraz R.sum, R.add, R.inc, R.negate, zeby
// - zsumowac array [2,3]
// - dodac do niego 3
// - zwiekszyc o jeden oraz zanegowac
//
// Poprawny wynik: -9
const newNumber = (list) => R.pipe(R.sum, R.add(3), R.inc, R.negate)(list);
newNumber([2, 3]);

// Cwiczenie 3:
// ============
// Uzyj R.compose(), R.prop(), oraz R.head(), zeby zwrocic imie pierwszej osoby.
const nameOfTheFirstPerson = (list) => R.compose(R.prop("name"), R.head)(list);
nameOfTheFirstPerson(PEOPLE); // => 'John'

// Cwiczenie 4:
// ============
// Uzyj R.compose(), R.map(), R.inc(), oraz R.filter(), zeby postarzec kazda z osob o 1 rok,
// i zwrocic tylko tych, ktorzy maja 25 lat lub mniej.
const onlyTwentyFiveYearOlds = (list) =>
  R.compose(
    R.filter((p) => p.age <= 25),
    R.map((p) => {
      let p2 = { ...p };
      p2.age = R.inc(p.age);
      return p2;
    })
  )(list);

onlyTwentyFiveYearOlds(PEOPLE);

// Cwiczenie 5:
// ============
// Uzyj R.map() do stworzenia nowego array na bazie PEOPLE array, ktorego kazdy
// element bedzie wygladal tak: [name, age] (np. ["John", 10]).
const newPeople = (list) => R.map(({ name, age }) => [name, age])(list);
newPeople(PEOPLE);

// Cwiczenie 6:
// ============
// Uzyj R.pipe, R.innerJoin(), i R.reduce() oraz R.add() do stworzenia nowego array
// na bazie PEOPLE array.
// 1. R.innerJoin() powinien zbudowac nowy array jedynie z osob o wieku 10, 25, i 54.
// 2. Uzyj R.reduce() i R.add(), zeby zsumowac wiek ludzi.
//
// Poprawny wynik: 89
const summedUpAges = (people, chosenOnes) => {
  return R.pipe(
    R.innerJoin((p, age) => p.age === age, people),
    R.map((p) => p.age),
    R.reduce((acc, age) => R.add(acc, age), 0)
  )(chosenOnes);
};

summedUpAges(PEOPLE, [10, 25, 54]);

// Cwiczenie 7:
// ============
// Powtorz cwiczenie nr 5, z jednym wyjatkiem. Zamiast uzywac R.reduce() i R.add(),
// uzyj R.sum() do zsumowania.
//
// Poprawny wynik: 89
const summedUpAges2 = (people, chosenOnes) => {
  return R.pipe(
    R.innerJoin((p, age) => p.age === age, people),
    R.map((p) => p.age),
    R.sum
  )(chosenOnes);
};

summedUpAges2(PEOPLE, [10, 25, 54]);

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
arr1 = R.take(3, NUMS);
arr2 = R.pipe(R.takeLast(3), R.reverse)(NUMS);
R.equals(arr1, arr2);
```

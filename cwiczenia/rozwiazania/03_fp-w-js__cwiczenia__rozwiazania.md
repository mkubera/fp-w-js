# Funkcyjne programowanie w JavaScript

Cwiczenia. Rozwiazania.

```javascript
// Dane:
// =====
var PEOPLE = [
  {name: "John", age: 10},
  {name: "Jack", age: 20},
  {name: "Jake", age: 25},
  {name: "Jesse", age: 39},
  {name: "Jill", age: 54}
]

// Cwiczenie 1:
// ============
// Uzyj R.compose() do uproszczenia ponizszej funkcji.
var theirName = (people) => R.compose(R.prop('name'), R.last)(people)
var theirName = (people) => {
	let composedFn = R.compose(R.prop('name'), R.last)
	return composedFn(people)
}
theirName(PEOPLE)  // => 'Jill'

// Cwiczenie 2:
// ============
// Uzyj R.compose(), R.prop(), oraz R.head(), zeby zwrocic imie pierwszej osoby.
var nameOfTheFirstPerson = R.compose(R.prop('name'), R.head)(PEOPLE)  // => 'John'

// Cwiczenie 3:
// ============
// Uzyj R.compose(), R.map(), R.inc(), oraz R.filter(), zeby postarzec kazda z osob o 1 rok, 
// i zwrocic tylko tych, ktorzy maja 25 lat lub mniej.
var onlyTwentyFiveYearOlds = R.compose(
	R.filter(p => p.age <= 25), 
	R.map(p => { 
		let p2 = {...p}; 
		p2.age = R.inc(p.age); 
		return p2
	})
)(PEOPLE)

// Cwiczenie 4:
// ============
// Uzyj R.map() do stworzenia nowego array na bazie PEOPLE array, ktorego kazdy 
// element bedzie wygladal tak: [name, age] (np. ["John", 10]).
var newPeople = R.map(({name,age}) => [name,age])(PEOPLE)

// Cwiczenie 5:
// ============
// Uzyj R.pipe, R.innerJoin(), i R.reduce() oraz R.add() do stworzenia nowego array 
// na bazie PEOPLE array.
// 1. R.innerJoin() powinien zbudowac nowy array jedynie z osob o wieku 10, 25, i 54. 
// 2. Uzyj R.reduce() i R.add(), zeby zsumowac wiek ludzi.
//
// Poprawny wynik: 89
var summedUpAges = R.pipe(
  R.innerJoin((p, age) => p.age === age, PEOPLE), 
  R.map(p => p.age), 
  R.reduce((acc, age) => R.add(acc, age), 0)
)([10,25,54])

// Cwiczenie 6:
// ============
// Powtorz cwiczenie nr 5, z jednym wyjatkiem. Zamiast uzywac R.reduce() i R.add(),
// uzyj R.sum() do zsumowania.
//
// Poprawny wynik: 89
var summedUpAges2 = R.pipe(
  R.innerJoin((p, age) => p.age === age, PEOPLE), 
  R.map(p => p.age), 
  R.sum
)([10,25,54])

// Cwiczenie 7:
// ============
// Uzyj R.pipe() oraz R.sum, R.add, R.inc, R.negate, zeby
// - zsumowac array [2,3]
// - dodac do niego 3
// - zwiekszyc o jeden oraz zanegowac
//
// Poprawny wynik: -9
var newNumber = R.pipe(R.sum, R.add(3), R.inc, R.negate)([2,3])


```
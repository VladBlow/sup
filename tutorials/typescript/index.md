---
title: Введение в TypeScript
date: 2016-04-03 17:18:22
---

<!-- toc -->

Скрипты Superpowers написаны на TypeScript. Это приятный в использовании, мощный язык с замысловатой историей.

Вот небольшой бэкграунд и введение, чтобы ввести вас в курс.

## Из JavaScript в TypeScript

JavaScript - это [изворотливый, дефектный язык](https://www.destroyallsoftware.com/talks/wat), который недавно стал намного лучше.
На самом деле, по ряду причин, он стандартизирован под именем ECMAScript.
Последняя версия JavaScript, ECMAScript 2015 (ранее известна, как ECMAScript 6), исправила множество разочарований людей и добавила много новых возможностей.

Путь всех этих улучшений до браузеров [может занять год или два](https://kangax.github.io/compat-table/es6/).
Тем временем, Веб-разработчики создали различные инструменты, например [Babel.js](https://babeljs.io/)
или языки [TypeScript](http://www.typescriptlang.org/), [Flow](http://flowtype.org/) и [CoffeeScript](http://coffeescript.org/), которые позволяют использовать новые возможности языка прямо сейчас и автоматически переписывают ("переводят") ваши скрипты в ECMAScript 5, версию JavaScript поддерживаемую, в данный момент, браузерами.

## Почему TypeScript?

В частности мы выбрали TypeScript, потому что это - введенное надмножество последней версии JavaScript. Это означает две вещи:

- При создании игр в Superpowers, миллионы Веб-разработчиков могут снова использовать свое, с трудом заработанное, знание JavaScript.
- Мощная и быстрая система типов TypeScript позволяет Superpowers поддерживать сообщения об ошибках в реальном времени и авто-завершение.

![](http://i.imgur.com/vnJU8Tt.gif)

Как дополнительный бонус, разработчики TypeScript работают с комитетом стандартов, чтобы информировать проектирование будущих версий JavaScript, что с любой точки зрения является большими инвестициями.

## Блок-контекстные переменные

<div class="note">
Если вы еще не знаете, что такое переменные или функции, вам вероятно нужно для начала [изучить основы JavaScript](https://learn.javascript.ru/first-steps).
</div>

ES2015 позволяет вам объявлять блок-контекстные переменные с помощью нового ключевого слова - `let`.
Как правило, вам больше не понадобится использовать старое ключевое слово `var`, которое имеет менее удобную, функционально-контекстную семантику.

```ts
// myVariable здесь не существует

if (someCondition) {
  // myVariable не может быть доступна здесь (перед ее объявлением)
  let myVariable = 10;
  // myVariable существует, пока блок не будет закрыт
}

// myVariable здесь тоже не существует
// Все эти комментарии будут ложными, если вы будите использовать `var`.
```

## Типизация переменных и функций

При объявлении переменной, TypeScript может определить ее тип основываясь на значении, которое вы ей присвоили:

```ts
let myVariable = 10; // у myVariable тип `number`

// TypeScript вернет ошибку, если вы сделаете что-то не правильно,
// например сложите number (число) и string (строку) 
myVariable += "строка";
```

Вы можете явно определить тип переменной, даже если не инициализировали её сразу:

```ts
let myVariable: string; // myVariable имеет тип `string`, и `undefined` как первоначальное значение
```

При объявлении функции, вы можете определить тип ее параметров и тип, который она возвращает:

```ts
function repeat(what: string, times: number): string {
  let result = "";
  for (let i = 0; i < times; i++) result += what;
  return result;
} 

let coolThrice = repeat("cool", 3); // Это вернет "coolcoolcool"
repeat(3, "cool"); // Ошибка, из-за неверных типов аргументов
```

Во многих случаях TypeScript также может вывести тип возврата функции на основе возвращенного значения.

Superpowers даже сможет авто-завершить `repeat` для вас и вывести на экран его подписи (параметры и тип возврата). 

[Читайте больше о типах](http://metanit.com/web/typescript/2.1.php).

## Классы

В ES5 было только наследование основанное на прототипе, которое довольно гибкое, но трудное для записи и чтения.
ES2015 добавляет классы для более традиционного объектно-ориентированного подхода, а TypeScript увеличивает их еще раз с помощью мощной проверки типа.

Вот, взгляните на синтаксис:

```ts
class Lift {

  // TypeScript позволяет вам объявлять участников и их типы
  floor: number;
  
  // Вы можете использовать значения параметров по умолчанию
  constructor(initialFloor=1) {
    this.floor = initialFloor;
  }
  
  goUp() {
    if (this.floor < 10) this.floor++;
  }
  
  goDown() {
    if (this.floor > 0) this.floor--;
  }
}

let myLift = new Lift();
myLift.goUp(); // теперь myLift.floor равен 2
```

[Читайте больше о классах в TypeScript](http://metanit.com/web/typescript/3.1.php).

## Улучшенные анонимные функции

В ECMAScript 5, при передаче функции как обратный вызов к другой функции, вам приходилось немного потанцевать с 'Function.bind', чтобы удостовериться, что значение 'this' было сохранено.

С ECMAScript 2015 вы можете использовать новый синтаксис толстой стрелки `() =>` для создания анонимных функций ("лямбда"), которые сохраняют `this` автоматически.

```ts
// Предположим, мы находимся в некотором контексте, где `this` имеет свойство `saving`.
this.saving = true;

// В этом примере 'saveToDisk' был бы продолжительной асинхронной функцией
saveToDisk(() => {
  // `this` все тот же. С `function() { ... }`, у вас не было бы гарантий 
  this.saving = false;
});
```

С ECMAScript 2015 классы и методы так же сохраняют значение `this` для вас.

[Читать больше о лямбдах](http://metanit.com/web/typescript/2.3.php).

## Перебор массивов

Другим давним больным местом JavaScript является перебор массивов.

```ts
let myArray = [ 100, 10, 1 ];

// С новым ключевым словом `of` в ECMAScript 2015, это просто
for (let value of myArray) console.log(value);

// До этого вы писали так:
// for (var i = 0; i < myArray.length; i++) console.log(myArray[i]);
```

## Читайте дальше

#### Английский

- [Codecademy's JavaScript course](http://www.codecademy.com/en/tracks/javascript)
- [Learning ES6](https://github.com/ericdouglas/ES6-Learning)
- [es6-features.org](http://es6-features.org/)
- [TypeScript Handbook](http://www.typescriptlang.org/Handbook)

#### Русский

- [Современный учебник JavaScript](https://learn.javascript.ru/)
- [Видео "Основы JavaScript" от Sorax](https://www.youtube.com/playlist?list=PL363QX7S8MfSxcHzvkNEqMYbOyhLeWwem)
- [Руководство по JavaScript](http://metanit.com/web/javascript/)
- [Руководство по TypeScript](http://metanit.com/web/typescript/)

| | |
|---|---|
| [> Оригинал](http://docs.superpowers-html5.com/en/tutorials/typescript-primer) | [Клавиши, мыш, геймпады... >](http://pajamdev.github.io/sup/tutorials/input) |

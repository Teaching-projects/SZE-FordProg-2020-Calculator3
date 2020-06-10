# JavaScript Calculator with Ohm Parser

Ez a projekt egy egyszerű bal rekurzív számológép Javascript nyelven, amelynek célja az Ohm Parser használatának megismerése és gyakorlása volt.

## Az Ohm Parser használata

# Böngésző

A legegyszerűbb módja az Ohm használatának böngészőben, ha közvetlenül az alábbi  hivatkozást adjuk hozzá - script tag segítségével - a weboldalunkhoz: 

https://unpkg.com/ohm-js@0.13.1/dist/ohm.min.js

A szkript meghívása lehetővé teszi az ohm elnevezésű globális változó használatát.

# Node.js

Az Ohm Node.js-ben történő használatához először az alábbi npm csomag telepítése szükséges:

npm install ohm-js

Ezt követően a require() segítségével hívhatjuk meg a szkriptben az Ohm-ot az alábbi módon:

var ohm = require('ohm-js');


## A számológép használata

A HTML fájl böngészőn keresztül történő megnyitását követően a számológép használatra kész.

Az input mezőben adható meg a kívánt kifejezés, a Parse gombra kattintva a parser végighalad a megadott kifejezésen és az eredményt a "Result:" felirat alatt jeleníti meg.

A számológép bal rekurzív, nem tesz különbséget a végrehajtható műveletek között, ezért a műveleti sorrend megadásához zárójelek használata szükséges.


## Az Ohm Grammar megadása

A szükséges nyelvtant legegyszerűbben egy script tag-en belül, a https://github.com/harc/ohm/blob/master/doc/syntax-reference.md hivatkozáson található Syntax Reference szerint adhatjuk meg.

# Példa:

Arithmetic {
  Exp = AddExp
  AddExp = AddExp "+" -- plus
  number = digit+
  }

## Parsolás

A parsolást a nyelvtanban rögzített szabályok alapján, minden szabályra érvényes művelet definiálásával hajthatjuk végre.

# Példa:

var g = ohm.grammarFromScriptElement();
 var semantics = g.createSemantics().addOperation('eval', {
 Exp: function(e) {
      return e.eval();
    },
 AddExp: function(e) {
      return e.eval(); 
    },
 AddExp_plus: function(left, op, right) {
      return left.eval() + right.eval();
    },
 number: function(chars) {
      return parseInt(this.sourceString, 10);
    },
 });

var result;

var input = '1 + 2';

var m = g.match(input);
 if (m.succeeded()) {
   result = semantics(m).eval();  // Evaluate the expression.
 } else {
   result = m.message;  // Extract the error message.
 }

## Források

https://github.com/harc/ohm
https://nextjournal.com/dubroy/ohm-parsing-made-easy
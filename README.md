# devit-test

Данные: 

```javascript
let testData = [1, 2, 1990, 85, 24, "Vasya", "colya@example.com", "Rafshan", "ashan@example.com", true, false];
let testData2 = [1, 2, 1990, 85, 24, 5, 7, 8.1];
let testData3 = [
   {
      name: "Vasya",
      email: "vasya@example.com",
      age: 20,
      skills: {
         php: 0,
         js: -1,
         madness: 10,
         rage: 10,
      },
   },
   {
      name: "Dima",
      email: "dima@example.com",
      age: 34,
      skills: {
         php: 5,
         js: 7,
         madness: 3,
         rage: 2,
      },
   },
   {
      name: "Colya",
      email: "colya@example.com",
      age: 46,
      skills: {
         php: 8,
         js: -2,
         madness: 1,
         rage: 4,
      },
   },
   {
      name: "Misha",
      email: "misha@example.com",
      age: 16,
      skills: {
         php: 6,
         js: 6,
         madness: 5,
         rage: 2,
      },
   },
   {
      name: "Ashan",
      email: "ashan@example.com",
      age: 99,
      skills: {
         php: 0,
         js: 10,
         madness: 10,
         rage: 1,
      },
   },
   {
      name: "Rafshan",
      email: "rafshan@example.com",
      age: 11,
      skills: {
         php: 0,
         js: 0,
         madness: 0,
         rage: 10,
      },
   },
];
let testData4 = [
   {
      name: "Vasya",
      email: "vasya@example.com",
      age: 20,
   },
   {
      name: "Dima",
      email: "dima@example.com",
      age: 34,
   },
   {
      name: "Colya",
      email: "colya@example.com",
      age: 46,
   },
   {
      name: "Misha",
      email: "misha@example.com",
      age: 16,
   },
   {
      name: "Ashan",
      email: "ashan@example.com",
      age: 99,
   },
   {
      name: "Rafshan",
      email: "rafshan@example.com",
      age: 11,
   },
   1,
   2,
   1990,
   85,
   24,
   "Vasya",
   "colya@example.com",
   "Rafshan",
   "ashan@example.com",
   true,
   false,
   [
      [
         [
            [
               1,
               2,
               1990,
               85,
               24,
               "Vasya",
               "colya@example.com",
               "Rafshan",
               "ashan@example.com",
               true,
               false,
               [
                  {
                     name: "Rafshan",
                     email: "rafshan@example.com",
                     age: 11,
                  },
               ],
            ],
         ],
      ],
   ],
];
```

## 1 (1бал) 
Сделать функцию поиска значений в массиве.

```javascript
function array_find(arr, str) {
   let resultsArr = [];

   function findFunc(arr, str) {
      for (let i = 0; i < arr.length; i++) {
         let isObject = !!arr[i] && arr[i].constructor === Object;
         let isArray = !!arr[i] && arr[i].constructor === Array;

         if (isObject) findFunc(Object.values(arr[i]), str);
         else if (isArray) findFunc(arr[i], str);
         else {
            if (str.constructor == RegExp) {
               if (str.test(arr[i])) resultsArr.push(arr[i]);
            } else {
               if (str == arr[i]) resultsArr.push(arr[i]);
            }
         }
      }
   }
   findFunc(arr, str);

   return resultsArr.length === 0 ? null : resultsArr;
}

const task1str = /@/;
let task1 = array_find(testData4, task1str);
console.log(task1);
```

## 2 (1бал)
Сделать функцию подсчета среднего значения, с возможностью исключения не числовых значений

```javascript
function array_avg(arr, allowNaN) {
   let sum = 0,
      times = 0;

   function iterateFunc(arr, allowNaN) {
      for (let i = 0; i < arr.length; i++) {
         let isObject = !!arr[i] && arr[i].constructor === Object;
         let isArray = !!arr[i] && arr[i].constructor === Array;

         if (isObject) iterateFunc(Object.values(arr[i]));
         else if (isArray) iterateFunc(arr[i]);
         else {
            let num = +arr[i];
            if (isNaN(num)) {
               if (allowNaN) times++;
               continue;
            }
            sum += num;
            times++;
         }
      }
   }
   iterateFunc(arr, allowNaN);

   return Math.round((sum * 100) / times) / 100;
}

let task2result = array_avg(testData2); // ~265
let task2result2 = array_avg(testData); // ~420
let task2result3 = array_avg(testData, true); // ~191
console.log(task2result);
console.log(task2result2);
console.log(task2result3);
```

## 3 (1бал)
Сделать функцию которая разбивает массив на подмассивы указанной длинны.

```javascript
function array_chunk(arr, chunkLength) {
   let newArr = [];

   for (let i = 0; i < arr.length; i += chunkLength) {
      let chunk = arr.slice(i, i + chunkLength);
      newArr.push(chunk);
   }

   return newArr;
}

let task3result = array_chunk(testData, 5);
let task3result2 = array_chunk(testData2, 5);
console.log(task3result);
console.log(task3result2);
```

## 4 (1бал) 
Сделать функцию которая обрезает массив до указанного значения.

```javascript
function array_skip_until(arr, value) {
   let sliceStart;

   for (let i = 0; i < arr.length; i++) {
      if (arr[i] === value) {
         sliceStart = i;
         break;
      } else {
         sliceStart = arr.length;
      }
   }

   return arr.slice(sliceStart);
}

let task4result = array_skip_until(testData, 2) // [2, 1990, 85, 24, "Vasya", "colya@example.com", "Rafshan", "ashan@example.com", true, false]
let task4result2 = array_skip_until(testData, "Rafshan") // ["Rafshan", "ashan@example.com", true, false]
let task4result3 = array_skip_until(testData, "asd") // []

console.log(task4result);
console.log(task4result2);
console.log(task4result3);
```

## 5 (1бал)
Сделать функцию для проверки существования значения в не нормализированном списке данных.

```javascript
function array_contains(arr, searchQuery) {
   let result = false;
   for (let i = 0; i < arr.length; i++) {
      let isObject = !!arr[i] && arr[i].constructor === Object;
      let isArray = !!arr[i] && arr[i].constructor === Array;

      if (isObject) array_contains(Object.values(arr[i]), searchQuery);
      else if (isArray) array_contains(arr[i], searchQuery);
      else {
         if (searchQuery.constructor == RegExp) {
            if (searchQuery.test(arr[i])) return !result;
         } else {
            if (searchQuery == arr[i]) return !result;
         }
      }
   }
   return result;
}

let task5result = array_contains(testData, 1990); // true
let task5result2 = array_contains(testData4, /^raf.*/i); // true
let task5result3 = array_contains(testData4, /^azaza.*/i); // false
console.log(task5result);
console.log(task5result2);
console.log(task5result3);
```


## 6 (1бал)
делать функцию для получения данных с массивов по указанному пути (аминь).
```javascript
function array_get(obj, string) {
   string = string.replace(/\[(\w+)\]/g, ".$1");
   string = string.replace(/^\./, "");
   let pathsArr = string.split(".");
   for (let i = 0; i < pathsArr.length; ++i) {
      let k = pathsArr[i];
      if (k in obj) {
         obj = obj[k];
      } else {
         return;
      }
   }
   return obj;
}

let result = array_get(testData4, "[5].name");
let result2 = array_get(testData4, "[17][0][0][0][11][0]");
let result3 = array_get(testData4, "[17][0][0][0][11][0][name]");

console.log(result); // "Rafshan"
console.log(result2); // {name: "Rafshan", email: "rafshan@example.com", age: 11}
console.log(result3); // "Rafshan"
```



## 7 (1бал)
Сделать функцию для поиска значений и пути к нему в не нормализированном списке данных (аминь).
```javascript
function array_search(arr, str) {
   let resultsArr = [],
      path = [];

   function findFunc(arr, str) {
      for (let i = 0; i < arr.length; i++) {
         let isObject = !!arr[i] && arr[i].constructor === Object;
         let isArray = !!arr[i] && arr[i].constructor === Array;

         if (isObject) findFunc(Object.values(arr[i]), str);
         else if (isArray) findFunc(arr[i], str);
         else {
            if (str.constructor == RegExp) {
               if (str.test(arr[i])) {
                  path.push(i);
                  resultsArr.push(arr[i]);
               }
            } else {
               if (str == arr[i]) {
                  path.push(i);
                  resultsArr.push(arr[i]);
               }
            }
         }
      }
   }
   findFunc(arr, str);

   return [resultsArr.length !== 0 ? resultsArr : null, path];
}

let task7result = array_search(testData4, /^raf.*/i); // [["[5].name", "Rafshan"], ["[13]", "Rafshan"], ["[17][0][0][0][7]", "Rafshan"], ["[17][0][0][0][11][0].name", "Rafshan"]]

console.log(task7result);
```



## 8 (1бал)
Создать функцию которая создает объект на основании двух представленных массивов используя один как ключи, а другой как значения. Не подходящие ключи массивов должны быть исключены.
```javascript
function array_combine(arrKeys, arrValues) {
   let obj = {};

   for (let i = 0; i < arrKeys.length; i++) {
      let key = arrKeys[i];
      let value = arrValues[i];

      if (!(typeof key == "string" || typeof key == "number")) continue;

      obj[key] = value;
   }

   return obj;
}

let task8result = array_combine(testData, testData2);
console.log(task8result); // {1: 1, 2: 2, 1990: 1990, 85: 85, 24: 24, "Vasya": 5, "colya@example.com": 7, "Rafshan": 8.1, "ashan@example.com": undefined}
```

## 10 (1бал)
Сделать функцию которая сможет делать срез данных с ассоциативного массива.

```javascript
function array_pluck(arr, path) {
   let pluckArray = [];
   let pathsArray = path.split(".");

   for (let obj of arr) findFunc(obj);

   function findFunc(obj) {
      for (let key in obj) {
         if (pathsArray[0] == key) {
            if (pathsArray[1]) {
               //repeat
               let innerObj = obj[key];
               for (let key in innerObj) {
                  if (pathsArray[1] == key) {
                     if (pathsArray[2]) {
                        //repeat
                     } else {
                        pluckArray.push(innerObj[key]);
                     }
                  }
               }
            } else {
               pluckArray.push(obj[key]);
            }
         }
      }
   }

   return pluckArray;
}

let task10result = array_pluck(testData3, "name");
let task10result2 = array_pluck(testData3, "skills.php");
console.log(task10result); // ["Vasya", "Dima", "Colya", "Misha", "Ashan", "Rafshan"]
console.log(task10result2); // [0, 5, 8, 6, 0, 0]
```


## 11 (1бал)
Сделать функцию которая возвращает уникальные элементы массива.

```javascript
function array_unique(array) {
   return [...new Set(array)];
}

let task11result = array_unique(testData.concat(testData2));
console.log(task11result); // [1, 2, 1990, 85, 24, 5, 7, 8.1, "Vasya", "colya@example.com", "Rafshan", "ashan@example.com", true, false]

```


## 12 (1бал)
Сделать функцию которая создает массив указанной длинны и заполняет его переданными значениями.

```javascript
function array_fill(length, string) {
   let newArr = [];

   for (let i = 0; i < length; i++) {
      newArr.push(string);
   }

   return newArr;
}

let task12result = array_fill(5, "string");
console.log(task12result); // ['string', 'string', 'string', 'string', 'string']
```


## 19 (1 бал)
Вывести в консоль по 4 значения из переданного массива с интервалом в 2 секунды.

```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8, "Vasya", "|", "123", 9, 10, 11, 12, 13, 14, 15];

function consoleLogInterval(arr) {
   arr.forEach((value, index) => {
      setTimeout(() => {
         console.log(value);
      }, 2000 * index);
   });
}

consoleLogInterval(arr);
```


## 21 (1 бал)
На основании данных testData3 вывести последовательно в консоль имена программистов сгруппированных и отсортированных по их навыкам:

```javascript
function sortBySkills(arr) {
   let codersSortedBySkills = {};

   let skillsList = ["php", "js", "madness", "rage"];

   skillsList.forEach((skill) => codersBySkill(skill));

   function codersBySkill(skill) {
      let tempArr = [...arr];
      tempArr.sort((a, b) => {
         a = a.skills[skill];
         b = b.skills[skill];

         if (a > b) return -1;
         if (a == b) return 0;
         if (a < b) return 1;
      });

      namesSorted = [];
      tempArr.forEach((coder) => {
         namesSorted.push(coder["name"]);
      });
      codersSortedBySkills[skill] = namesSorted;
   }

   return codersSortedBySkills;
}

let task21result = sortBySkills(testData3);

for (let skill in task21result) {
   console.log(`-----${skill.toUpperCase()}-----`);
   task21result[skill].forEach((name, index) => {
      console.log(`${index + 1}. ${name}`);
   });
}
```










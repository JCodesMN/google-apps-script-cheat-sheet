# Google Apps Script Cheat Sheet

##[General](#general)
* [Array](#array)
  * [Check for a Value](#check-for-a-value)
  * [Remove Duplicates](#remove-duplicates)
  * [Remove Empty](#remove-empty)
  * [Get Count of Values in Array](#get-count-of-values-in-array)
  * [Intersect of Two Arrays](#intersect-of-two-arrays)
* [Array of Objects](#array-of-objects)
  * [Sort by Property or Properties](#sort-by-property-or-properties)
  * [Find Object With Unique Property Value](#find-object-with-unique-property-value)
  * [Filter by Property Value](#filter-by-property-value)
* [Object](#object)
  * [Array of Matching Property Values](#array-of-matching-property-values)
* [Dates and Times](#dates-and-times)
  * [Formatted Timestamps](#formatted-timestamps)
  * [Match a Date to a Range](#match-a-date-to-a-range)

## General

### Array

#### Check for a Value
  ➡  boolean
  return `boolean`

```javascript
function checkValIn(arr, val) { 
	return arr.indexOf(val) > -1; 
}

var arr_cvi = [1,2,3,4];

if (checkValIn(arr_cvi, 99)) {
    Logger.log("check value in ➡ 99 is in the array"); 
  } else {
    Logger.log("check value in ➡ 99 is not in the array");
}

```

#### Remove Duplicates
  ➡  array

```javascript
function rmDuplicatesFrom(arr) {
	var check = {};
	var _arr  = [];
	var j     = 0;
	for(var i = 0; i < arr.length; i++) {
		var item = arr[i];
		if(check[item] !== 1) {
			check[item] = 1;
			_arr[j++] = item;
		}
	}
	return _arr;
}

var arr_rdf = [1,2,3,1,2,3,4,];
var ex_rdf  = rmDuplicatesFrom(arr_rdf);
Logger.log("rmDup input ➡ " + arr_rdf);
Logger.log("rmDup output ➡ " + ex_rdf);
```

#### Remove Empty Values
  ➡ array

```javascript

function rmEmptyVal(x){
	return (x !== (undefined || ''));
}

var arr_rev = ["a",,"b",,,"c"];
var ex_rev  = arr_rev.filter(rmEmptyVal);
Logger.log("rmEmpty input ➡ " + arr_rev);
Logger.log("rmEmpty output ➡ " + ex_rev);
```

#### Get Count of Values in Array
  ➡ array of objects 

```javascript
function countOfValIn(arr){
	var _arr = [];
	var copy = arr.slice(0);
	for (var i = 0; i < arr.length; i++) {
		var myCount = 0;	
		for (var w = 0; w < copy.length; w++) {
			if (arr[i] == copy[w]) {
				myCount++;
				delete copy[w];
			}
		}
		if (myCount > 0) {
			var a = new Object();
			a.value = arr[i];
			a.count = myCount;
			_arr.push(a);
		}
	}
	return _arr;
}

var arr_covi = ["A", "B", "C", "A", "B", "C", "D", "A"];
var ex_covi  = countOfValIn(arr_covi);
Logger.log("countVal input ➡ " + arr_covi);
Logger.log("countVal out ⬇ ");
Logger.log(ex_covi);
```

#### Intersect of Two Arrays
  ➡ array

```javascript
function intersectOf(arrA, arrB) {
	var a    = 0;
	var b    = 0;
  var _arr = [];
  while( a < arrA.length && b < arrB.length ) {
     if (arrA[a] < arrB[b] ) { a++; }
     else if (arrA[a] > arrB[b] ) { b++; }
     else {
       _arr.push(arrA[a]);
       a++;
       b++;
     }
  }
  return _arr;
}

var arr1_io = [1, 2, 3];
var arr2_io = [3, 4, 5];
Logger.log(intersectOf(arr1_io, arr2_io));
```

### Array of Objects

#### Example Array
```javascript
var ex_arrObj = [
{a: 1000, b: 1, c: 5}, 
{a: 10000, b: 2, c: 5000}, 
{a: 10, b: 2, c: 500},
{a: 1, b: 1, c: 50}
]
```

#### Sort by Property or Properties

```javascript
function dynSort(prop) {
	var sortOrder = 1;
	if(prop[0] === "-") {
		sortOrder = -1;
		prop = prop.substr(1);
	}
	return function (a,b) {
		var result = (a[prop] < b[prop]) ? -1 : (a[prop] > b[prop]) ? 1 : 0;
		return result * sortOrder;
	}
}

ex_arrObj.sort(dynSort("a"));
Logger.log("arrObj sorted by 'a' value ⬇ ");
Logger.log(ex_arrObj);

function dynSortM() {
	var props = arguments;
	return function (obj1, obj2) {
		var i = 0, result = 0, numberOfProperties = props.length;
		while(result === 0 && i < numberOfProperties) {
			result = dynSort(props[i])(obj1, obj2);
			i++;
		}
		return result;
	}
}

ex_arrObj.sort(dynSortM("b", "c"));
Logger.log("arrObj sorted by 'b' and 'c' values ⬇ ");
Logger.log(ex_arrObj);
```

#### Find Object With Unique Property Value

```javascript
function findObjIn(arrObj, pQuery, val) {
	for (var i = 0; i < arrObj.length; i++) {
		var obj = arrObj[i];
		for (var prop in obj) {
			if (obj.hasOwnProperty(pQuery) && prop == pQuery && obj[prop] == val) {
				return obj;
			}
		}
	}
}


Logger.log("find obj with 'a' value of 1000 ⬇ ");
var ex_foi = findObjIn(ex_arrObj, "a", 1000);
Logger.log(ex_foi);

function findObjValIn(arrObj, pQuery, val, pReturn) {
	for (var i = 0; i < arrObj.length; i++) {
		var obj = arrObj[i];
		for (var prop in obj) {
			if (obj.hasOwnProperty(pQuery) && prop == pQuery && obj[prop] == val) {
				return obj[pReturn];
			}
		}
	}
}

Logger.log("find obj with 'c' value of 500 and return its 'a' value ⬇ ");
var ex_fovi = findObjValIn(ex_arrObj, "c", 500, "a");
Logger.log(ex_fovi);

```

#### Filter by Property Value

```javascript

```

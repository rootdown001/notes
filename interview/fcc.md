## Find the Symmetric Difference

The mathematical term symmetric difference (△ or ⊕) of two sets is the set of elements which are in either of the two sets but not in both. For example, for sets A = {1, 2, 3} and B = {2, 3, 4}, A △ B = {1, 4}.

Symmetric difference is a binary operation, which means it operates on only two elements. So to evaluate an expression involving symmetric differences among three elements (A △ B △ C), you must complete one operation at a time. Thus, for sets A and B above, and C = {2, 3}, A △ B △ C = (A △ B) △ C = {1, 4} △ {2, 3} = {1, 2, 3, 4}.

```jsx
function sym(...args) {
  // console.log(args);

  function accumulator(arr1, arr2) {
    const temp = [];

    arr1.forEach((e) => {
      if (arr2.indexOf(e) === -1 && temp.indexOf(e) === -1) {
        temp.push(e);
      }
      // console.log("temp(one): ", temp);
    });

    arr2.forEach((e) => {
      if (arr1.indexOf(e) === -1 && temp.indexOf(e) === -1) {
        temp.push(e);
      }
      // console.log("temp(two): ", temp);
    });
    return temp;
  }

  const result = args.reduce(accumulator);
  console.log("🚀 ~ file: App.jsx:10 ~ sym ~ result:", result);

  console.log(result);
  return result;
}

sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]);
```

## Inventory Update

Compare and update the inventory stored in a 2D array against a second 2D array of a fresh delivery. Update the current existing inventory item quantities (in arr1). If an item cannot be found, add the new item and quantity into the inventory array. The returned inventory array should be in alphabetical order by item.

```jsx
function updateInventory(arr1, arr2) {
  // arr2.forEach((item2) => {
  for (let item2 of arr2) {
    var found = false;

    for (let item1 of arr1) {
      // arr1.forEach((item1) => {
      if (item2[1] === item1[1]) {
        item1[0] = item1[0] + item2[0];
        found = true;
        break;
      }
    }
    if (found === false) {
      arr1.push(item2);
    }
  }

  arr1.sort((a, b) => a[1].localeCompare(b[1]));

  console.log(arr1);
  return arr1;
}
// Example inventory lists
var curInv = [
  [21, "Bowling Ball"],
  [2, "Dirty Sock"],
  [1, "Hair Pin"],
  [5, "Microphone"],
];

var newInv = [
  [2, "Hair Pin"],
  [3, "Half-Eaten Apple"],
  [67, "Bowling Ball"],
  [7, "Toothpaste"],
];

updateInventory(curInv, newInv);
```

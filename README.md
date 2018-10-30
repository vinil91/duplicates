# duplicates task

## Task
Example:
```
const exampleInput = [1, 5, 8, 5, 1001, 3, 1, 44, 5, 3];
onlyDuplicates(exampleInput);
=> [1, 5, 3]

returns new array only with duplicates of input array
```

## Solution 1.
```
export const onlyDuplicates = input => {
  input.sort();
  const duplicates = input.filter((element, index, array) => {
    const notFirstNumberInInput = index !== 0;
    const notSameValueOnRight = array[index + 1] !== element;
    const isSameValueOnLeft = array[index - 1] === element;
    return notFirstNumberInInput && notSameValueOnRight && isSameValueOnLeft;
  });
  return duplicates;
};

```

The idea is to filter sorted array, when we traverse it from left to right. The output array will be composed of the rightmost entry of duplicate value in sorted input array.
If we will include not only the rightmost duplicate, value will appear in output array more than once. It's not our case.

For the element of sorted array to be "the RIGHTMOST DUPLICATE", 3 conditions must to be met:

1. To be the rightmost DUPLICATE, the element shouldn't be the first. When the element is the first, we know only about the first entry of this value. It couldn't be a DUPLICATE.
```
  const notFirstNumberInInput = index !== 0;
```
2. To be the rightmost DUPLICATE, the element should have element with the same value on his left.
```
  	const isSameValueOnLeft = array[index - 1] === element;
```
3. To be a RIGHTMOST duplicate, the element shouldn't have an element with the same value on his right.
```
	const notSameValueOnRight = array[index + 1] !== element;
```
The elements from input array, which meet all conditions are selected in output array

## Solution 2.
```
export const onlyDuplicates = input => {
  let amountOfNumberEntriesInArray = input.reduce((accumulator, currentValue) => {
    if (!accumulator.hasOwnProperty(currentValue)) {
      const firstAppearance = { [currentValue] : 1};
      return Object.assign(accumulator, firstAppearance);
    } else {
      const increaseOfEntries = { [currentValue]: accumulator[currentValue] + 1};
      return Object.assign(accumulator, increaseOfEntries);
    }
  }, {});
  return Object.keys(amountOfNumberEntriesInArray)
    .map(item => parseInt(item))
      .filter((key, index) => amountOfNumberEntriesInArray[key] > 1);
};
```
The idea is to filter array, when we traverse it from left to right and reduce it to the the object, where property keys are elements value, and property values are amount of value entries in input array.
```
  let amountOfNumberEntriesInArray = input.reduce((accumulator, currentValue) => {
    if (!accumulator.hasOwnProperty(currentValue)) {
      const firstAppearance = { [currentValue] : 1};
      return Object.assign(accumulator, firstAppearance);
    } else {
      const increaseOfEntries = { [currentValue]: accumulator[currentValue] + 1};
      return Object.assign(accumulator, increaseOfEntries);
    }
  }, {});
```

Next, we select such keys whose property values are greater than one, that occurs in array more than one time.
```
  return Object.keys(amountOfNumberEntriesInArray)
    .map(item => parseInt(item))
      .filter((key, index) => amountOfNumberEntriesInArray[key] > 1);
};
```

## Solution 3.
```
export const onlyDuplicates = input => {
  const uniqueValues = [];
  input.foreach(inputItem => {
    if (uniqueValues.indexOf(inputItem) === -1) {
      uniqueValues.push(inputItem);
    }
  });

  let duplicates = uniqueValues.filter(uniqueItem => {
    let count = 0;
    input.forEach(inputItem => {
      if (inputItem === uniqueItem) {
        count++;
      }
    });
    return count > 1;
  });
  return duplicates;
};
```

Firstly, we shape the array of unique values from the input string.
```
export const onlyDuplicates = input => {
  const uniqueValues = [];
  input.foreach(inputItem => {
    if (uniqueValues.indexOf(inputItem) === -1) {
      uniqueValues.push(inputItem);
    }
  });
```
Secondly, we traverse an array of unique Values, and count, how often values has found in the array.
```
  let duplicates = uniqueValues.filter(uniqueItem => {
    let count = 0;
    input.forEach(inputItem => {
      if (inputItem === uniqueItem) {
        count++;
      }
    });
```
If it was found more than once, we put in the output array.
```
    return count > 1;
```

# Thank you for your attention!

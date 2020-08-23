```js
function MaxPair(list) {
  let index = 0
  let n = list.length
  let temp,count1=0,count2=0
  for(let i=0;i<n;i++){
    if(list[i]>list[index]) index=i
    count1++
  }
  temp = list[index]
  list[index] = list[n-1]
  list[n-1] = temp
  index = 0
  for(let i=0;i<n-1;i++){
    if(list[i]>list[index]) index=i
    count2++
  }
  temp = list[index]
  list[index] = list[n-2]
  list[n-2] = temp
  console.log('total comparision: ',count1+count2)
  return list
}

MaxPair([2,1,3,4,9,8])
```

```js
function mergeSort (unsortedArray) {
  // No need to sort the array if the array only has one element or empty
  if (unsortedArray.length <= 1) {
    return unsortedArray;
  }
  // In order to divide the array in half, we need to figure out the middle
  const middle = Math.floor(unsortedArray.length / 2);

  // This is where we will be dividing the array into left and right
  const left = unsortedArray.slice(0, middle);
  const right = unsortedArray.slice(middle);
  console.log('mergesort: ',left,right)
  // Using recursion to combine the left and right
  return merge(
    mergeSort(left), mergeSort(right)
  );
}

// Merge the two arrays: left and right
function merge (left, right) {
  console.log('merge start ',left,right)
  let resultArray = [], leftIndex = 0, rightIndex = 0;

  // We will concatenate values into the resultArray in order
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      resultArray.push(left[leftIndex]);
      leftIndex++; // move left array cursor
    } else {
      resultArray.push(right[rightIndex]);
			rightIndex++; // move right array cursor
    }
    console.log('merge ',resultArray)
  }

  // We need to concat to the resultArray because there will be one element left over after the while loop
  return resultArray
          .concat(left.slice(leftIndex))
          .concat(right.slice(rightIndex));
}


mergeSort([3,1,2,6,-1,0,10])
```

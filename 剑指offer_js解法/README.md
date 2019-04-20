## 剑指offer
#### 1.在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```javascript
function Find(target,array) {
  var row = array.length - 1,i,j;
  for(i=row,j=0;i>=0 && j<array[i].length;) {
    if(array[i][j] === target) {
      return true;
    }else if(array[i][j] < target) {
      j++;
      continue;
    }else if(array[i][j] > target) {
      i++;
      continue;
    }
  }
  return false;
}
```

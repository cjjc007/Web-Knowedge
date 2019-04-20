## 剑指offer——javascript版本
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
#### 2.请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```javascript
function replaceSpace(str) {
  return str.replace(/\s+/g, '%20');
}
```
#### 3.输入一个链表，/*从尾到头*/打印链表每个节点的值。
```javascript
function printListFromTailToHead(head) {
  if(!head) {
    return 0;
  }else {
    var arr=[];
    while(head!=null){
        arr.push(head.val);
        head=head.next;
    }
    return arr.reverse()
  }
}
```

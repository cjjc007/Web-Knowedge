## 剑指offer——javascript版本
#### 1.在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```javascript
function Find(target, array) {
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
#### 4.输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
function reConstructBinaryTree(pre, vin) {
  
}



#### 5.用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

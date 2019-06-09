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
    return arr.reverse();
  }
}
```
#### 4.输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
```javascript
function reConstructBinaryTree(pre, vin) {
  if(pre.length == 0 || vin.length == 0){
        return null;
  }
  index = vin.indexOf(pre[0]);
  left = vin.slice(1, index);
  right = vin.slice(index+1);
  var node = new TreeNode(vin[index]);
  node.left = reConstructBinaryTree(pre.slice(0,index),left);
  node.right = reConstructBinaryTree(pre.slice(index+1),right)
  return node;
}
```

#### 5.用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
```javascript
var result=[];
function push(node){
  stack1.push(node);
}
function pop() {
  return result.shift()；
}
```

#### 6.把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
```javascript
function minNumberInRotateArray(rotateArray){
    var len = rotateArray.length;
    if(len === 0){
        return 0;
    }
    return Math.min.apply(null,rotateArray);
}
```
#### 7.大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39
```javascript
function Fibonacci(n){
    var result=[];
    if(n<=0) return 0;
    else if(n<=2) return 1;
    else{
        result[1]=1;
        result[2]=2;
        for(var i=3;i<=n;i++){
            result[i]=result[i-1]+result[i-2];
        }
        return result[n-1]
    }
}
```
#### 8.一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
```javascript
function jumpFloor(number)
{
    if(number==1) return 1;
    if(number==2) return 2;
    var n1=1;
    var n2=2;
    var result=0;
    for(var i=3;i<=number;i++){
        result=n1+n2;
        n1=n2;
        n2=result;
    }
    return result;
}
```
#### 9.一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
```javascript
function jumpFloorII(number){
  return Math.pow(2, number - 1);
}
```
#### 10.我们可以用21的小矩形横着或者竖着去覆盖更大的矩形。请问用n个21的小矩形无重叠地覆盖一个2* n的大矩形，总共有多少种方法？
```javascript
function rectCover(number)
{
    if(number==1) return 1;
    if(number==2) return 2;
    var n1=1;
    var n2=2;
    var result=0;
    for(var i=3;i<=number;i++){
        result=n1+n2;
        n1=n2;
        n2=result;
    }
    return result;
}
```
#### 11.输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
```javascript
function NumberOf1(n) 
  { 
      if(n<0){
          n = n>>>0;
      }
      var res = n.toString(2); 
      var count = 0; 
      for(var i = 0; i <res.length; i++){ 
          if(res[i] == 1){ 
              count++  
          } 
      } 
      return count; 
  }
```
#### 12.给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
```javascript
function Power(base, exponent){
  return Math.pow(base, exponent);
  //return base**exponent;
}
```
#### 13.输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
```javascript
function reOrderArray(array){
  var arr1=[], arr2=[];
  array.map(function(i){
        i%2==0?arr2.push(i):arr1.push(i);
    })
    return arr1.concat(arr2);
}
```
#### 14.输入一个链表，输出该链表中倒数第k个结点。
```javascript
function FindKthToTail(head, k)
{
    if(head==null||k<=0) return null;
    var prev = head;
    var tail = head;

    for(var index=0;index<k-1;index++){
        if(tail.next!=null){
            tail=tail.next;
        }else{
            return null;
        }        
    }    
    while(tail.next!=null){
        prev=prev.next;
        tail=tail.next;
    }
    return prev;
}
```
#### 15.输入一个链表，反转链表后，输出链表的所有元素。
```javascript
function ReverseList(pHead){
    var newHead, temp;
    if(!pHead){
        return null;
    }
    if(pHead.next === null){
        return pHead;
    } else {
        newHead = ReverseList(pHead.next);
    }
    temp = pHead.next;
    temp.next = pHead;
    pHead.next = null;
    temp = null;
    return newHead;
}
```
#### 16.输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
```javascript
unction ListNode(x){
    this.val = x;
    this.next = null;
}
function Merge(pHead1, pHead2)
{
    var head=new ListNode(0);
    var pHead=head;
    while(pHead1!=null && pHead2!=null){
        if(pHead1.val>=pHead2.val){
            head.next=pHead2;
            pHead2=pHead2.next;
        }else{
            head.next=pHead1;
            pHead1=pHead1.next;
        }
        head=head.next;
    }
    if(pHead1!=null){
        head.next=pHead1;
    }
    if(pHead2!=null){
        head.next=pHead2;
    }
    return pHead.next;
}
```
#### 17.输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）
```javascript
function HasSubtree(pRoot1, pRoot2){
    if(pRoot1 == null || pRoot2 == null){
        return false;
    }
    if(isSubTree(pRoot1, pRoot2)){
        return true;
    } else{
        return HasSubtree(pRoot1.left, pRoot2) || HasSubtree(pRoot1.right, pRoot2);
    }
    function isSubTree(pRoot1, pRoot2){
        if(pRoot2 == null) return true;
        if(pRoot1 == null) return false;
        if(pRoot1.val === pRoot2.val){
            return isSubTree(pRoot1.left, pRoot2.left) && isSubTree(pRoot1.right, pRoot2.right);
        } else {
            return false;
        }
    }
}
```

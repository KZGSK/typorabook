# ACM模式输入输出

## import

~~~java
import java.util.*;
~~~

## 框架

~~~java
public class Main{
    public static void main(String[] args){
        
    }
}
~~~

## 输入

~~~java
Scanner sc = new Scanner(System.in);
while(sc.hasNextLine()){//存在下一行数据
    int n = nextInt();//此行的第一个数字
    String[] gradeArr = sc.nextLine().split(" ");//读取一行的数据
}
~~~

## 输出

~~~java
System.out.println(res + "\n" );//结果并空行
System.out.println(String.format("%.2f", sum / gradeArr.length));//保留两位小数

~~~

## 链表

~~~java
public static class LinkedNode{
        int val;
        LinkedNode next;
        public LinkedNode(){}
        public LinkedNode(int val){
            this.val = val;
        }
    }
~~~

## 二叉树

~~~java
class TreeNode {
    String val;
    TreeNode left;
    TreeNode right;

    public TreeNode(String x) {
        this.val = x;
        this.left = null;
        this.right = null;
    }
public TreeNode makeTree(String pre, String mid){//建树 固定格式背下来
        if(pre.isEmpty()){
            return null;
        }
        
        char root = pre.charAt(0);
        int rootIndex = mid.indexOf(root);
        
        String leftpre = pre.substring(1, rootIndex + 1);
        String leftmid = mid.substring(0, rootIndex);
        String rightpre = pre.substring(rootIndex + 1);
        String rightmid = mid.substring(rootIndex + 1);
        
        TreeNode left = makeTree(leftpre, leftmid);
        TreeNode right = makeTree(rightpre, rightmid);
        
        return new TreeNode(root, left, right);
    }
~~~


# leeetcode 1017

## 题解

不得不说这是一道初见杀的题目……因为你会下意识去想组合求解。

但是这个题目的核心思路（也是唯一思路）还是采用进制转换，只不过需要加一点特殊处理

**核心思路：使用传统进制转换将十进制数变为带-1，0，1的进制位数，然后将-1，0，1的串变为0，1的串**

这个思路第一眼看肯定很难理解，我下面结合例子说明：

如果按传统方法将3转化为-2进制，将会是这样一个过程：
$$
3\%-2=1\space
$$

$$
3/-2=-1
$$

那么我们得到了当前的位数：-1，具体怎么用它，我们后面会讲

类似的，我们计算：
$$
-1\%-2=-1
$$

$$
-1/-2=0
$$

然后用传统进制转换的方法，将其反转，我们就得到了：
$$
-11
$$
这样一个数列。

尽管这个数列不是题目需要的，但是它的确是3在-2进制的另外一种表达：
$$
(-1)\times(-2)^{1}+(1)\times(-2)^0=3
$$
那么，既然我们最后的序列中不需要-1,如何将-1去除呢？

答案是**把-1拆成当前的1和更上一位的1**,证明如下：
$$
(-1)\times(-2)^n=1\times(-2)^{n+1}+1\times(-2)^n
$$
那么刚才那个例子中，
$$
-11
$$
就变成了
$$
111
$$
我们可以验证一遍：
$$
1\times(-2)^2+1\times(-2)^1+1\times(-2)^0
$$
显然等于3。

## AC代码

```javascript
/**
 * @param {number} N
 * @return {string}
 */
var baseNeg2 = function(N) {
    let res="";
    while(N){
        let mod=N%(-2);
        N=N/(-2)>0?Math.floor(N/(-2)):Math.ceil(N/-2);
        if(mod<0){
            mod+=2;
            N+=1;
        }
        res+=mod.toString();
    }
    if(res==='')res='0'
    
    return reverse(res);
};

var reverse=function(str){
    let arr=[]
    for(let i=str.length-1;i>=0;i--){
        arr.push(str.charAt(i));
    }
    return arr.join('');
}
```

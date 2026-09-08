设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

**示例：**

```C++
输入： arr = [1,3,5,7,2,4,6,8], k = 4
输出： [1,2,3,4]
```

思路：依旧分三段，每一段元素个数分别为a b c ,比较k和a b c的大小关系，分类讨论

![e461bab9-2796-4a14-8722-1a6b5c771174.png](images/e461bab9-2796-4a14-8722-1a6b5c771174.png)

无需对每段进行**精细**排序

```C++
class Solution {
public:
    vector<int> smallestK(vector<int>& arr, int k) 
    {
        if(k == 0)
        return {};
        qsort(arr,0,arr.size()-1,k);
        return vector<int>(arr.begin(),arr.begin()+k);    
    }
    void qsort(vector<int>& arr,int l,int r,int k)
    {
        //1.确定基准元素
        int mid = getmid(arr,l,r);
        //2.将数组分为三块
        int left = l-1,right = r+1,i = l;
        int pivot = arr[mid];
        while(i < right)
        {
            if(arr[i] < pivot)
            swap(arr[++left],arr[i++]);
            else if(arr[i] == pivot)
            i++;
            else
            swap(arr[--right],arr[i]);
        }
        //3.分三种情况讨论
        int a = left - l + 1,b = right -left - 1;
        if(a >= k)
        qsort(arr,l,left,k);
        else if(a+b >= k)
        return;
        else
        qsort(arr,right,r,k-a-b);
    }
    int getmid(vector<int>& arr,int left,int right)
    {
         int mid = left + (right - left) / 2;
        if ((arr[left] >= arr[mid] && arr[mid] >= arr[right]) ||
            (arr[right] >= arr[mid] && arr[mid] >= arr[left])) 
        return mid;
        else if ((arr[mid] >= arr[left] && arr[left] >= arr[right]) ||
                   (arr[right] >= arr[left] && arr[left] >= arr[mid])) 
        return left;
        else 
        return right;
    }
    
};
```

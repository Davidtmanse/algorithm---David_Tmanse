#### 快速排序 - C++ | 算法的学习目前我都是使用C++来实现的。

```
#include <iostream>

using namespace std;

const int N = 100010;
int a[N];

void quike_sort(int a[] , int l , int r)
{
    if (l >= r) return;

    int i = l - 1 , j = r + 1 , x = a[l + r >> 1];

    while (i < j)
    {
        do i ++ ; while (a[i] < x);
        do j -- ; while (a[j] > x);
        if (i < j) swap(a[i] , a[j]);
    }

    quike_sort(a , l , j); // 对分好后的左边进行再排
    quike_sort(a , j + 1 , r); // 对分好后的右边进行再排
}

int main()
{
    int n;
    cin >> n;

    for (int i = 0 ; i < n ; i ++) scanf("%d" , &a[i]);

    quike_sort(a , 0 , n - 1);

    for (int i = 0 ; i < n ; i ++) printf("%d " , a[i]);
    return 0;

}
```

### Java实现版本
```
import java.util.Scanner;

public class Main{

    private static int N = 1000010;
    private static int[] q = new int[N];
    
    public static void quike_sort(int[] q, int l, int r){
        if (l >= r) return;
        
        int i = l - 1, j = r + 1, x = q[l + r >> 1];
        
        while (i < j){
            do i ++; while (q[i] < x);
            do j --; while (q[j] > x);
            if (i < j){
                int t = q[i];
                q[i] = q[j];
                q[j] = t;
            }
        }
        
        quike_sort(q, l, j);
        quike_sort(q, j + 1, r);
    }
    
    
    
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt();
        
        for (int i = 0; i < n; i ++){
            q[i] = sc.nextInt();  
        }
        
        quike_sort(q, 0, n - 1);
        
        for (int i = 0; i < n; i ++)
        {
            System.out.print(q[i] + " ");
        }
        
        
    }
}
```

1.`main`函数作为主函数，实现数据输入到进入快排函数的入口。
2.`quike_sort`函数实现数组排序的功能

对于`int a[N]`数组在外部，不能够很好的去进行使用吧。
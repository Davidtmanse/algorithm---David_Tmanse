##### 归并排序 - C++

```
#include <iostream>

using namespace std;

const int N = 1e5 + 10;

int q[N], tmp[N];

void merge_sort(int q[], int l, int r)
{
	if (l >= r) return;

	int mid = l + r >> 1;

	merge_sort(q, l, mid), merge_sort(q, mid + 1, r);

	int k = 0, i = l , j = mid + 1;
	while (i <= mid && j <= r) 
		if (q[i] <= q[j]) tmp[k ++] = q[i ++];
		else tmp[k ++] = q[j ++];

	while (i <= mid) tmp[k ++] = q[i ++];
	while (j <= r) tmp[k ++] = q[j ++];

	for (i = l , j = 0 ; i <= r; i ++ , j ++) q[i] = tmp[j];
}


int main()
{
	int n;
	scanf("%d",&n);

	for (int i = 0 ; i < n ; i ++) scanf("%d",&q[i]);

	merge_sort(q, 0, n - 1);

	for (int i = 0 ; i < n ; i ++) printf("%d ",q[i]);

	return 0;
}
```

1.`main`函数实现数据的输入操作
2.`merge_sort`函数实现归并排序

可见，这种模板的写法虽然能够实现归并排序，但是使用的效果还是不够优美。毕竟有数组竟然是全局的。真正的工程应用也许并不足够实用。
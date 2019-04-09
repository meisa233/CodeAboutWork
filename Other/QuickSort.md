基于C++实现的<br />
```C++
using namespace std;

void quickSort(int *arr, int left, int right) {
	int i = left;
	int j = right;
	int temp;
	int pivot = arr[(left + right) / 2];
	while (i <= j) {
		while (arr[i] < pivot)
			i++;
		while (arr[j] > pivot)
			j--;
		if (i <= j) {
			temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
			i++;
			j--;
		}
	}
	if (left < j)
		quickSort(arr, left, j);
	if (i < right)
		quickSort(arr, i, right);
}

int main()
{
	int list[] = { 7, 1, 34, 16, 12, 44, 19, 99, 0, 4 };
	quickSort(list, 0, 9);
	return 0;
		
}
```

#### High_Presicion

```
//add
vector<int> add(string a, string b){
	vector<int> A, B, sum;
    for (int i = a.length() - 1; i >= 0; i--) A.push_back(a[i] - '0');
    for (int i = b.length() - 1; i >= 0; i--) B.push_back(b[i] - '0');
    int t = 0;
    for (int i = 0; i < A.size() || i < B.size() || t; i++) {  //这里的判断条件巧妙增加最后一次进位
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        sum.push_back(t % 10);  //在得到每位的值再push可避免值溢出
        t /= 10;
    }
    return sum;
}
//sub
vector<int> sub(string a, string b){  //其实等价于abs() 因为我在函数内部判断a和b的大小
	vector<int> A, B, diff, zero(1, 0);
	int isABigger = -1;
	for (int i = a.length() - 1; i >= 0; i--) A.push_back(a[i] - '0');
    for (int i = b.length() - 1; i >= 0; i--) B.push_back(b[i] - '0');
    if(A.size() == B.size()){
		for(int i = A.size()-1; i >= 0; i--){
			if(A[i] == B[i]) continue;
			if(A[i] > B[i]){
				isABigger = 1;
				break;
			}
			if(A[i] < B[i]){
				isABigger = 0;
				break;
			}
		}
	} else if(A.size() > B.size()) isABigger = 1;
	else isABigger = 0;
	if(isABigger == -1) return zero;
    if(isABigger){
    	B.resize(A.size());
    	for(int i = 0; i < A.size(); i++){
			if(A[i]>=B[i]){
				diff.push_back(A[i]-B[i]);
			} else{
				A[i+1]--;
				diff.push_back(A[i]+10-B[i]);
			}
		}
    } else{
    	A.resize(B.size());
    	for(int i = 0; i < B.size(); i++){
			if(B[i]>=A[i]){
				diff.push_back(B[i]-A[i]);
			} else{
				B[i+1]--;
				diff.push_back(B[i]+10-A[i]);
			}
		}
    }
    for(int i = diff.size()-1; i >= 0; i--){
    	if(diff[i] == 0) diff.pop_back();
    	else break;
    }
    return diff;
}
//mul
vector<int> mul(string a, string b){
	vector<int> A, B, prod;
	for(int i = a.length()-1; i >= 0; i-- ) A.push_back(a[i] - '0');
	for(int i = b.length()-1; i >= 0; i-- ) B.push_back(b[i] - '0');
	int more = 0;
	for(int i = 0; i < B.size(); i++){
		vector<int> prod_row(i);
		for(int j = 0; j < A.size() || more; j++){
			if(j < A.size()) more += A[j] * B[i];
			prod_row.push_back(more % 10);
			more /= 10;
		}
		int t = 0;
		vector<int> new_prod;
		for(int j = 0; j < prod_row.size() || j < prod.size() || t; j++){
			if(j < prod_row.size()) t += prod_row[j];
			if(j < prod.size()) t += prod[j];
			new_prod.push_back(t%10);
			t /= 10;
		}
		prod = new_prod;
	}
	return prod;
}
//div (a/b) 需提前判断 b != 0
vector<int> div(string a, string b){
	vector<int> A, B, quot;  //quotient 商
	//通过减法模拟 试商法
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	string a, b;
	cin >> a >> b;
	vector<int> sum = sub(a, b);  //例
	for(int i = sum.size()-1; i >= 0; i--){
		cout << sum[i];
	}
	cout << endl;
	system("pause");
	return 0;
}
```

**注** 上述只是事例 强烈建议让上述所有函数的输入和输出都保持vector型

**注** int a = '9' - '1'; 此处减的是char的ASCII，巧妙得自动转为类型为int的a

**注** char achar = 'A'; 处理十六进制时，int aint = achar - 'A' + 10;

#### Recursion

**注** 在函数内调用自身即为递归 函数不一定需要返回值

#### Skill

当我们不确定循环上(下)限时 可尝试while一直循环找到则break 慎用



#### QuickSort

```
//这样声明参数能减少不必要麻烦
void quickSort(//数组, int left, int right);
```


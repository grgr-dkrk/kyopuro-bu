## 動的計画法(DP) 導入

### DP とは
問題を複数に分割し、結果をメモしながら解いていく手法。

### フィボナッチ数列の DP
`n - 2` と `n - 1` を足した結果が解となる。  
また、`n - 2` と `n - 1` もそれぞれの `n - 2` と `n - 1` を見る。  
再帰的に求める必要があるので、普通に書くとこうなる↓

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX 10010

int fib(int n) {
  if (n == 0 || n == 1) return n;
  return fib(n - 1) + fib(n - 2);
}

int main() {
  int N;
  cin >> N;
  cout << fib(N) << endl;
}
```

しかしこの方法は `n - 2` と `n - 1` の `fib()` の計算結果に依存するので、毎回計算することになり、[指数関数的](./complexity#指数関数時間)に計算量が膨れ上がる。  
そこで、既に計算済みの結果を記録しながらやれば、[多項式時間](./complexity#多項式時間)の O(N) となる。

以下のように `dp` 配列（というか一次元のテーブル）を用意し、各要素に `fib()` の結果を入れれば、`dp[n]` に値がある場合の n の結果を計算しなくていい。  
つまり、フィボナッチ数列という問題を小分けして、それぞれの結果をメモしていくことになる。  
結果 から `n <= 1` まで向かうので、トップダウン方式と呼ばれる。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX 10010

int dp[MAX] = {0};

int fib(int n) {
  if (n == 0 || n == 1) return n;
  if (dp[n]) return dp[n];
  return dp[n] = fib(n - 1) + fib(n - 2);
}

int main() {
  int N;
  cin >> N;
  cout << fib(N) << endl;
}
```

漸化式っぽく見ると、`fib(0)` は 0, `fib(1)` は 1, それ以外は `dp[n] = dp[n - 1] + dp[n - 2]` になるので、再帰関数でやらずとも、ループを回して式の結果を記録していくだけでいい。  
結果に向かっていくので、ボトムアップ形式と呼ばれる。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX 10010

int dp[MAX] = {0, 1}; // n = 0, n = 1 のケースは既に決まっているので入れておく

int main() {
  int N;
  cin >> N;
  for (int i = 2; i <= N; i++) dp[i] = dp[i - 1] + dp[i - 2]; // 回すだけ
  cout << dp[N] << endl;
}
```

トップダウン | ボトムアップ形式それぞれの計算量は同じ O(N)。

### 0-1 ナップサック問題の DP
```
0-1 ナップサック問題
N 個の荷物は、それぞれ重さ（weight）と価値（value）が決まっている。
重さの合計が W を超えないよう、価値の合計が最大となるように荷物をナップサックに入れる時、その価値はいくつか。
```

フィボナッチ数列は一次元の情報を見るだけで良かった。  
ナップサック問題は価値に加えて「最大容量を超えない」情報も考慮しないといけない。  
2021/01/27 時点、ナップザック問題は [NP困難](./complexity#NP困難)なので、ひたすら調べるしか方法がない。  
最大容量ごとに積めるかどうか判定し、積める場合は価値を加えるというのをテーブルに記録していく。

以下はボトムアップ形式の場合。テーブルは TBD。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX_WEIGHT 100
#define MAX_VALUE 100
#define MAX_N 101
#define MAX_M 10010

int main() {
  int N, W;
  int weight[MAX_WEIGHT], value[MAX_VALUE];
  int dp[MAX_N][MAX_M]; // DP 用のテーブル
  cin >> N >> W; // 荷物の個数、ナップサックの最大重量を入力
  for (int i = 0; i < N; i++) cin >> weight[i] >> value[i]; // N 個の荷物それぞれの重さと価値を入力

  for (int i = 0; i < N; i++) {
    // 0 から W までの容量を想定したテーブルの設定
    for (int j = 0; j <= W; j++) { // 容量を j とした時のケースを回していく
      // dp[i + 1][j]のケースでの価値を決める
      // i 番目の重さが容量 j に収まる場合...
      // dp[i][容量 - i 番目の重さ] に i 番目の価値を加え、それを dp[i][j] のケースと比較して、大きい方をメモする
      // i 番目の重さが容量 j を超えている場合...
      // dp[i][j] を返す（前のループで返された結果か、初期値 0）
      dp[i + 1][j] = j >= weight[i] ? max(dp[i][j - weight[i]] + value[i], dp[i][j]) : dp[i][j];
    }
  }

  // dp[N][W] に価値が入る
  cout << dp[N][W] << endl; // 
}
```
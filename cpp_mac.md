## C++ 導入（Mac）

### はじめに
C++ は速い、静的なので人気。  

解くのに便利なライブラリを一括で読み込むために `bits/stdc++.h` というヘッダーファイルがある。  
使う人が多いので使う。

[で、結局 #include <bits/stdc++.h> って何? - Qiita](https://qiita.com/hakatashi/items/f9d9abf05a002b5c4dc5)

### コンパイラ
clang とか gcc とかある。  
`bits/stdc++.h` をインクルードできる gcc 一択だったりする。  
ただし clang の方が速い。そもそも gcc はマルチスレッドでコンパイルしない。  
`istream`, `ostream` など入出力に使うことがあるライブラリは、clang だと遅く、gcc だと速い。

[競技プログラミングにおける C++ の入出力を高速化する (入力編) - Qiita](https://qiita.com/nojima/items/57b9d39d7d73362ac883)

### インストール
XCode 導入済みの Mac で `which gcc` を叩くと、以下のように出力される。

```
$ which gcc
/usr/bin/g++
```

`gcc --version` を叩くと、以下のように出力される。  

```sh
$ gcc --version
Configured with: --prefix=/Applications/Xcode.app/Contents/Developer/usr --with-gxx-include-dir=/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
Apple clang version 12.0.0 (clang-1200.0.32.27)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```

Mac の gcc は自分を gcc と思い込んでいる clang なので、gcc を入れる必要がある。  
※ Xcode のビルドに影響がなく、仮にあったとしてもすぐ戻せるように alias で設定します

`brew` などで gcc をインストール。

```sh
brew install gcc
```

シェルの config で、g++ に alias を指定する。  
（g++-xx の xx の部分は変わるかも、インストール時に要確認）

```
# 普段使ってるシェルの設定
alias g++='/usr/local/bin/g++-10'
```

シェルを再起動する。  
`g++ --version` を叩いて下記が出力されれば OK。

```
$ g++ --version
g++-10 (Homebrew GCC 10.2.0_2) 10.2.0
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

### VSCode の設定
ローカルでコードを書くために VSCode の拡張機能と設定が必要。

1. [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) を入れる。
2. `settings.json` に `"C_Cpp.default.compilerPath": "/usr/local/bin/g++-10"` を追加。
3. [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner) を入れる。  
4. 設定画面から Code Runner の設定に移動し、`Run In Terminal` にチェックを付ける。（下図2参考）
5. VSCode をリロードする。

![Cmd + , で設定画面に入り、coderunner で検索、`Run In Terminal` にチェックを付ける](2021-01-12-12-22-53.png)

### やってみる
VSCode を起動し、`main.cpp` を作って以下をコピペ。  
（入力された数値を出すだけのコード）

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
  int N;
  cout << "Input:" << endl;
  cin >> N;
  cout << N << endl;
}
```

<kbd>control + option + n</kbd> を押して動けば OK。

#### NG: `#include <bits/stdc++.h>` に赤波線が出る
settings.json の `C_Cpp.default.compilerPath` で正しいパスが指定されていない。  

#### NG: コンパイル時に　`fatal error: 'bits/stdc++.h' file not found`　エラーになる
Clang でコンパイルしている。coderunner 自体は g++ を実行しているだけなので alias を確認。



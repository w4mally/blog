---
layout: post
title:  "WSLのg++をアップデートする"
---

競技プログラミングをやっているときに`std::map::contains`を使おうとしたらVScodeにそんなものはないと怒られた。<br>
そんな訳ないので調べてみると、まず`std::map::contains`はC++20から追加された機能であり、あまりにも環境構築が適当すぎてローカルのC++の環境がC++17であることが判明。<br>
というわけで手元でもC++20を使えるようにしました。<br>

## やったこと
環境:Ubuntu20.04.6(WSL)<br>
まず以下のコマンドを叩く。<br>
```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install -y g++-13
```
この後
```
g++-13 --version
```
を叩いてバージョンが表示されたらインストールはOK。<br>
ただしこのままだと手元にg++13が入っただけでデフォルトで使われるg++が変わらない。<br>
そこでまず以下のコマンドをそれぞれ叩いてg++の場所を確認する。
```
which g++   
which g++-13
```
そして
```
sudo update-alternatives --install (g++の場所) g++ (g++-13の場所) 13
```
とするとデフォルトで使われるg++がG++-13になる。<br>
```
g++ --version
```
を叩いて`g++ (Ubuntu 13.1.0-8ubuntu1~20.04.2) 13.1.0...`とか出たらうまくいっている。<br>
さらにRemoteWSLでVScodeを起動して`Ctrl + Shift + P`で「C/C++:構成の編集」を選択し、`compilerPath`と`cppStandard`の項目を
```
"compilerPath": (g++-13の場所),
"cppStandard": "c++20",
```
と変更。<br>
これでC++20からの機能をVScode上で書いても警告が出なくなる。<br>
最後にg++でコンパイルするときにデフォルトでC++20が使われるようにする(いちいち`g++ -std=c++20 hoge.cpp`と書くのは面倒くさい)。<br>
まずUbuntu上で
```
nano ~/.bashrc
```
などとして`.bashrc`を編集する(viでも問題ない)。<br>
`.bashrc`の末尾に
```
alias g++='g++ -std=c++20'
```
と追記して保存。<br>
コマンドライン上で
```
source ~/.bashrc
```
を叩けばOK。<br>
これでg++でコンパイルするときにデフォルトでC++20でコンパイルされる。<br>
上手くいかない場合はいったんUbuntuを再起動すれば大丈夫なはず<br>
<br>
というわけで(今更過ぎるけど)無事にC++20が手元でも使えるようになりました。<br>
なんかヤバいバージョン運用をしている気がするんですがC++は現状競プロでしか使ってないしとりあえずこれでいいでしょう。<br>
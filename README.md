卒論LaTex Readme
===

Install
---
### **TexLiveのインストール**
TeXを書くための環境として[TeXLive](https://texwiki.texjp.org/?TeX%20Live) をインストールします．
インストールについては[リンク先](https://tm23forest.com/contents/texlive2020-install-latex-begin)のサイト様が参考になります．
### **VSCodeのインストール**
TeXLiveだけでも書けるのですが，VSCodeを用いた方が数倍楽なのでこの方法をお勧めします．
まず初めに[VSCode](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)をインストールします．
インストール後にLaTe Workshopという拡張機能をいれます．

![](https://i.imgur.com/rmaJvSt.png)

Ctrl+,か左下の歯車マークを押して設定に移ります．右上の設定（JSON）を開くを押します．
settings.jsonに以下のコードを追加します．

```
    "latex-workshop.latex.tools": [
        {
            "command": "ptex2pdf",
            "args": [
                "-l",
                "-ot",
                "-kanji=utf8 -synctex=1",
                "%DOC%"
            ],
            "name": "Step 1: ptex2pdf"
        },
        {
            "command": "pbibtex",
            "args": [
                "%DOCFILE%",
                "-kanji=utf8"
            ],
            "name": "Step 2: pbibtex"
        },
        {
            "command": "ptex2pdf",
            "args": [
                "-l",
                "-ot",
                "-kanji=utf8 -synctex=1",
                "%DOC%"
            ],
            "name": "Step 3: ptex2pdf"
        },
        {
            "command": "ptex2pdf",
            "args": [
                "-l",
                "-ot",
                "-kanji=utf8 -synctex=1",
                "%DOC%"
            ],
            "name": "Step 4: ptex2pdf"
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "toolchain",
            "tools": [
                "Step 1: ptex2pdf",
                "Step 2: pbibtex",
                "Step 3: ptex2pdf",
                "Step 4: ptex2pdf"
            ]
        }
    ],
    "latex-workshop.view.pdf.viewer": "tab",
    "editor.renderControlCharacters": true,
    "window.zoomLevel": 1
```
その後TeXファイルを開き，TeXメニュからBuild LaTeX projectを押せばpdfが書き出されます．

How to Use
---
このプロジェクトをCloneして使ってください．
[必須ファイル](http://prdownloads.osdn.jp/mytexpert/26068/jlisting.sty.bz2) をダウンロードしてCloneしたLaTexフォルダに入れてください．

基本的に使い方はググってください．～～LaTexで検索すれば，いっぱい情報が出てきます．
以下では自分が卒論書く時に多用したものの解説を行います．
### **画像の挿入について**
画像の挿入のコードは次の物になります．

```
\begin{figure}[hb]
    \begin{center}  
        \includegraphics[width=0.725\linewidth]{Figure/kanagawa.pdf}
        \caption{神奈川工科大学\cite{kanakouPage}}
        \label{fig:kanakou}
    \end{center}
\end{figure}
```
冒頭のhbは出力位置の指定を意味します．
| h | here   | その位置   |
|---|--------|------------|
| t | top    | ページ上端 |
| b | bottom | ページ下端 |
| p | page   | 単独ページ |
3行目のwidth=数字については数字の部分の割合になっています．
横幅マックスを1(100%)とした時この場合は75%の大きさという事です．図のサイズは適宜変更してください．
{Figure/kanagawa.pdf}については画像の場所を示しています．
Figureフォルダのkanagawa.pdfを出力するという意味です．
caption内の\citeについては参考文献で説明します．
TeXでは画像や表，ソースコード，章などにラベルという変数のような物を作れます．
ここではラベルをfig:kanakouとし，本文中で`\ref{fig:kanakou}`と宣言すると図の番号がコンパイルされます．
表やプログラム，章も共通です．
### **表の挿入について**
表の挿入のコードは次の物になります．
```
\begin{table}[htbp]
    \centering
    \caption{ローマ字表です}
    \label{tab:Romazi}
    \begin{tabular}{l|cc}
      & a & k  \\ \hline
    a & a & Ka \\
    i & i & Ki \\
    u & u & Ku
    \end{tabular}
\end{table}
```
人間が見ても良くわかりません．そのため表の作成には[リンク先](https://www.tablesgenerator.com/)のサイトが便利です．
ウェブページ上で表の作成も行え，CSVファイルのインポートも可能です．
ここに出てくるcaption, labelは画像の挿入と同じ意味です．

### **ソースコードの挿入について**
Tex中でのソースコードの挿入についてはいくつか方法があります．
一つは本文中に直接ソースコードを書く方法です．
```
\begin{lstlisting}[caption=UUIDの変更関数,label=ChangeUUID, firstnumber=1]
    #include<stdio.h>
    void main(){
        printf("TeXファイルに直接ソースコードを書く場合です．")
    }
\end{lstlisting}
```
begin以下に実際にプログラムを書きます．beginの行にcaption, label, firstnumberがあります．
firstnumberはこのプログラムリストの行番号を何から始めるか，という事です．
例えば何百行もあるソースコードの一部を抜粋して書きたい時など，firstnumberの番号を変える事で参照が簡単になります．

もう一つの方法はソースファイルを参照し挿入する方法です．
```
\lstinputlisting[caption=SoturonTex,label=soturon]{Soturon.tex}
```
{Soturon.Tex}の部分でファイルを参照しています．
実際に運用する際は{Program/hogehoge.c}などにするのが良いと思います．
### **参考文献について**
参考文献の挿入について書く
参考文献はarticle.bibを編集します．以下はWebPageの場合のコードです．

```
@webpage{kanakouPage,
	author = "Kanagawa Institute of Technology.",
	title = "神奈川工科大学ホームページ",
	howpublished = "\url{https://www.kait.jp/}",
	note = {(参照日 2020-11-24)}
}
```
本文中で引用するには`\cite{kanakouPage}`書きます．
これはBibTeXというものを用いており，bibファイルに参考文献を書いていても本文中で引用していなければ参考文献リストに載りません．
また，bibファイルどの順番で書いていても引用した順番に変更してくれます．
（※設定で変更可能です．）

以下はGoogle ScholarでBibTeXコードを簡単に発行する方法です．
目的の論文の引用符を押します．
![](https://i.imgur.com/SIVDOyt.png)

![](https://i.imgur.com/bUyA8K0.png)
BibTeXを押す事でコードが表示されます．
論文掲載サイトには大体BibTeXの項目があります．本の引用の仕方はarticle.bibに載っているものか調べて適宜参照してください．

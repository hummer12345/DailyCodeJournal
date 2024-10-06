---
title: delegate
type: docs
---

## C#のdelegateとは

特定のシグネチャをもつメソッドへの参照を保持できる

## どういうことか

要するに一塊の処理を変数のように扱える  
コードを見るほう理解しやすい  
このコードで何が起きているかというと、  
1. メンバ変数として、戻り値なし、引数なしの「Print」という関数をdelegateを用いて定義している  
2. Form1_Shown関数内でメンバ変数として定義した「Print」関数に無名関数で処理を代入している  
3. 最後に、処理を代入した「Print」関数そのものを呼び出している

結果として、label1にdelegateという文字が表示される

```C#
using System;
using System.Windows.Forms;

namespace WindowsFormsApp
{
    public partial class Form1 : Form
    {
        delegate void Print();

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Shown(object sender, EventArgs e)
        {
            Print printNum = () => { label1.Text = "delegate"; };
            printNum();
        }
    }
}

```

シグネチャは自由に定義できる。  
以下はPrint関数に返り値と引数を定義した例  

```C#
using System;
using System.Windows.Forms;

namespace WindowsFormsApp
{
    public partial class Form1 : Form
    {
        delegate string Print(string arg1);
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Shown(object sender, EventArgs e)
        {
            Print printNum = (string arg1) => { return arg1 + " " + "delegate"; };
            label1.Text = printNum("引数だよ");
        }
    }
}

```

## マルチキャストデリゲート

先ほどの例では一つのデリゲートに一つのメソッドを定義していたが、実は複数メソッド定義することができる  
* += でメソッドを追加、-=でメソッドを削除でき,設定されたメソッドは順番に呼び出される  
* 返り値を持つメソッドが複数登録された場合、最後のメソッドのみ取り出される  
* 例外が起きると、以降のメソッドは実行されない  

```C#
using System;
using System.Windows.Forms;

namespace WindowsFormsApp3
{
    public partial class Form1 : Form
    {
        delegate void MathOperation(int a, int b);
        public Form1()
        {
            InitializeComponent();
        }

        private void Add(int x, int y)
        {
            label1.Text = (x + y).ToString();
        }

        private void Subtract(int x, int y)
        {
            label2.Text = (x - y).ToString();
        }

        private void Form1_Shown(object sender, EventArgs e)
        {
            MathOperation op = Add;
            op += Subtract;
            
            //label1に15,lable2に5と表示される
            op(10, 5);
        }
    }
}

```


## そもそもこんなことができて、何が嬉しいのか

### 柔軟なコールバック処理

```C#

```
### コードの再利用性と拡張性の向上

```C#

```
---
title: 拡張メソッド
type: docs
---


## 拡張メソッドとは

本来メソッドが追加できない既存クラスにメソッドを追加できる仕組み。

## サンプル

```C#
public static class IntExtensions
{
    private const int MOD = 998244353;

    // 余りを取る拡張メソッド
    public static int Mod(this int number)
    {
        int result = number % MOD;
        return result < 0 ? result + MOD : result;
    }
}

int b = 3;
Console.WriteLine(b.Mod()); //３と表示される
```

上では「既存クラスにメソッドを追加できる仕組み」と書いたが、実際は静的クラスを呼んでいる。  
利用者側から見ると、あたかもそのクラスがメソッドを持っているように見える。  
ライブラリなどにも適用でき、あくまで拡張であるため、既存のコードに手を加えることなく処理が追加できるのが魅力的。

## ユースケース
### 既存クラスへのメソッドの追加

```C#
public enum Status
{
    Started,
    InProgress,
    Completed
}


public static class EnumExtension
{
    public static string DisplayStatus(this Status status)
    {
        switch (status)
        {
            case Status.Started:
                return "開始";
            case Status.InProgress:
                return "進行中";
            case Status.Completed:
                return "完了";
            default:
                return "不明な状態";
        }
    }
}

Status.Completed.DisplayStatus()　//完了と表示される
```

### インタフェースの拡張
IShapeの実装クラスは共通してDisplayInfoメソッドを使用できる
```C#
using System;

namespace InterfaceExtensionExample
{
    public interface IShape
    {
        double GetArea();
    }

    public static class ShapeExtensions
    {
        public static void DisplayInfo(this IShape shape)
        {
            Console.WriteLine($"形の種類: {shape.GetType().Name}");
            Console.WriteLine($"面積: {shape.GetArea()}");
        }
    }
}
```

### 反復ステートメントの拡張

```C#
public static class EnumerableExtensions
{
    public static void ForEach<T>(this IEnumerable<T> collection, Action<T> action)
    {
        foreach (var item in collection)
        {
            action(item);
        }
    }
}

var numbers = new List<int> { 1, 2, 3 };
numbers.ForEach(n => Console.WriteLine(n));
```
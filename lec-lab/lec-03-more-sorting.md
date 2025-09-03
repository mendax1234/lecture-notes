# Lec 03 - More Sorting

## Stable Sort

A sorting algorithm is called **stable** if the relative order of elements **with the same key value** is preserved by the algorithm after sorting is performed.

### Customized Sorting

As in Java, usually the [#default-sorting](lec-02-sorting.md#default-sorting "mention") is **stable**, what is sometimes we need a **customized sorting**? For example, the following Kattis problem is a good example.

{% embed url="https://open.kattis.com/problems/sortofsorting" %}

To solve this problem, we need a customized sorting algo which will only sort based on the **first two characters** of the string. To implement the **customized sorting**, we can implement our own Comparator in `Collections.sort()` or `Arrays.sort()`.

{% code lineNumbers="true" %}
```java
// Read an unsorted Array
ArrayList<String> names = new ArrayList<>();
for (int i = 0; i < n; i++) {
  names.add(br.readLine());
}

// Sort it using customized sorting algo
Collections.sort(names, (a, b) -> {
  return a.substring(0, 2).compareTo(b.substring(0, 2));
});
```
{% endcode %}

This customized sorting is implemented using a [**lambda expression**](https://wenbo-notes.gitbook.io/cs2030s-notes/lec-rec-lab-exes/lecture/lec-08-functional-programming#lambda-expression) on the **functional interface** [`Comparator`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Comparator.html) in Java. In this case, `(a, b)` are the **parameters**, which matches the abstract method `compare`'s parameter list. And the R.H.S is the **computation**, in this case, the comparison.

{% hint style="success" %}
[CS2030S](https://wenbo-notes.gitbook.io/cs2030s-notes) Here! Great!
{% endhint %}

## Fast I/O

Instead of using the standard I/O [`Scanner`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Scanner.html) provided, we can use the fast I/O — [`BufferReader`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/io/BufferedReader.html). Still use the [Kattis problem above](lec-03-more-sorting.md#customized-sorting) as the background, to use `BufferReader` in the problem, we can write the following code,

{% code lineNumbers="true" %}
```java
import java.io.*;

public class sortofsorting {
  public static void main(String[] args) throws Exception {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    PrintWriter pw = new PrintWriter(System.out);
    
    // Read Input
    int n = Integer.parseInt(br.readLine());
    ArrayList<String> names = new ArrayList<>();
    for (int i = 0; i < n; i++) {
      names.add(br.readLine());
    }
    
    // Print
    for (String name : names) {
      pw.println(name);
    }
    pw.close();
  }
}
```
{% endcode %}

{% hint style="success" %}
#### Notes

1. In `BufferReader`, there is **no** [token-based input](https://wenbo-notes.gitbook.io/cs2040s-notes/java-basics-for-dsa#token-based-input-and-line-based-input), like `.nextInt()`, etc. There is only line-based input, so we need to parse the line by ourselves.
2. A quick look at what `pw.close()` does: Basically, `pw.println()` will print everything to the buffer first and once we call `pw.close()`, everything in the buffer will be printed.
{% endhint %}

## Quick Sort

The whole idea of quick sort is **pivot** and **three zones**:

1. pivot: in quick sort, it is always chosen to be the first index (index 0) of the partition
2. three zone: the partition is always divided into 3 zones
   1. the zone in which all the elements is **smaller** than the pivot
   2. pivot
   3. the zone in which all the elements are **greater** than the pivot

After each round, the **pivot** is put at the **correct** position. For visualization, go the goated website — [VisualAlgo](https://visualgo.net/en/sorting)!

### Random Quick Sort

It is a slight variation of quick sort, which picks the pivot **randomly**, and then **swap** the pivot with the first index element. Then the remaining are the same.

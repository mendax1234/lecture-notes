# Lec 02 - Sorting

## Default Sorting

In Java, the sorting is implemented in

1. [Collections](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html) (Use `Collections.sort(List)` to sort a List)
2. [Arrays](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html) (Use `Arrays.sort(array)` to sort an array, like `int[]`)

An example which uses this feature to solve a problem is as follows,

{% embed url="https://open.kattis.com/problems/nothanks" %}

Basically, we can create an array using `ArrayList` and then pass that array as the argument of the [`Collections.sort()`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-), after that the array will be sorted ascending order, according to the natural order of its elements.

{% code lineNumbers="true" %}
```java
public class nothanks {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    sc.nextLine();
    ArrayList<Integer> card = new ArrayList<>();
    for (int i = 0; i < n; i += 1) {
      card.add(sc.nextInt());
    }
    Collections.sort(card);
    int ans = card.get(0);
    for (int i = 1; i < n; i += 1) {
      if (card.get(i - 1) + 1 != card.get(i)) {
        ans += card.get(i);
      }
    }
    System.out.println(ans);
  }
}
```
{% endcode %}

# Implicit FSM Traversal Methods

## State Extraction

> This part is **not covered in detail** in NUS EE4218.

The basic idea of state extraction is rather easy

1. Find all states reachable from an **initial state**, and add these states to the subset of reachable states.
2. From this subset, determine all the reachable states, and add them to the subset.
3. Repeat the above step, and stop when [convergence is reached](#user-content-fn-1)[^1].

For example, if our initial state is `001` and we have 2 inputs, meaning that we have 4 combinations of input to apply on any state including the initial state `001`.

* Let's say after trying all 4 input combinations on the initial state `001`, we find out that the next state can be `010` or `100`. Thus, we add these two states to our set of reachable states from `001`. The set is now, `{001, 010, 100}`.
* Now, we apply all 4 input combinations on `010` and `100` **respectively** and see what are the states that we can achieve.
* We repeat this until we have traversed all the states in the set of reachable states.

{% hint style="success" %}
Up until now, [retiming](sequential-circuit-optimization-using-network-models.md#retiming-1), [state encoding](synchronous-circuit-optimization-using-state-based-models.md#state-encoding), and [state extraction](implicit-fsm-traversal-methods.md#state-extraction) can all be done by the **modern synthesis tool**. However, the [**state minimization**](synchronous-circuit-optimization-using-state-based-models.md#state-minimization) and the [repipelining](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-02/lec-02b-rtl-transformations#repipelining) we learned in EE4415 is done **manually**.
{% endhint %}

[^1]: No new reachable states added to the subset.

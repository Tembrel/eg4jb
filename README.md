# Asynchronous interdependent tasks
This repo has code demonstrating two different approaches to executing
tasks asynchronously when the some tasks depend on the completion of
others. The example here is of making a pizza using the following tasks:
1. Combining the ingredients of the dough.
1. Letting the dough rise.
1. Combining the ingredients of the sauce.
1. Grating the cheese.
1. Rolling out the risen dough into a crust.
1. Putting the sauce on top of the crust.
1. Putting the grated cheese on top of the sauce.

The DAG of dependencies looks like this:
```
1 --> 2 --> 5 --v
3 --------------+--> 6 --v
4 -----------------------+--> 7
```
The `LatchPizzaBuilder` approach uses CountDownLatches to prevent tasks from
proceeding until the tasks they depend on have completed.
This implementation reflects a more restrictive dependency graph than the one shown above:
It waits for 3, 4, and 5 to be ready before performing 6 and 7 together.

The `FuturePizzaBuilder` approach uses CompletableFutures to arrange for
the tasks to be performed in an order consistent with the DAG shown above.

Both cases use a fixed-size thread pool to run tasks asynchronously.

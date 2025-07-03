# DF2 Miner implementation in rust

## Important info

1. This uses jsonocel 1.0 object centric event logs (https://www.ocel-standard.org/1.0/).
2. After execution, the process tree is printed in the console. Everything else is logged in process.log file.
3. starts_cuts file implements the exact mathematical formula of Inductive miner, so it can be very slow if there are a large number of activities.
4. starts_cuts_opti file implements optimised algorithms for finding cuts in Inductive miner, so it is very fast.

## Optimised finding-cut algorithms 

### Sequence cut
1. It uses the dfg to create strongly connected group(SCC) of activities.
2. It considers the SCCs as a single activity and finds how they are connected(edges between them).
3. For 2 SCC a->b, all 'a's are put in set1, and 'b's in set2.
4. Remove duplicated SCCs  and put it into a seperate list.
5. For each activity 'c' in the duplicated list, and for every activity 's' in set1, if 's' can reach 'c' and 'c' cannot reach 's', put it in set2, else put it in set1. 
5. Convert the SCCs to their orginal activities group.
6. If either of the sets are empty, then sequence cut is not possible. If the entire graph is stringly connected, you will have only 1 SCC, and hence, this cut is not possible.

### Exclusive cut
1. Using dfg, find disjoint group of activities.
2. Put 1st group to set1 and rest to set2.
3. If either of the sets are empty, then exclusive cut is not possible.

### Parallel cut
1. Go through the activities 1 by 1.
2. Put the first activity in set1.
3. For an activity 'b', if it has to-and-from edges with every activity in set1, then put it in set2.
4. Else if, if it has to-and-from edges with every activity in set2, then put it in set1.
5. Else, return false as parallel cut is not possible.
6. Also, check the extra conditions for parallel cut to be sure.

### Redo cut
1. Put start and end activities into set1.
2. For the remaining activities, starting from a start activity, if it is possible to reach a remaining activity before reaching an end activity; and starting from an end activity, if it is not possible to reach the remaining activity before reaching a start activity, then put it in set1.
3. Starting from a start activity, if it is not possible to reach a remaining activity before reaching an end activity; and starting from an end activity, if it is possible to reach the remaining activity before reaching a start activity, then put it in set2.
4. Else, return false as redo cut is not possible.
5. Also, check the extra conditions for redo cut to be sure.
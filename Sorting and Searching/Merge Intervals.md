Given a collection of intervals, merge all overlapping intervals.

Example 1:

Input: `[[1,3],[2,6],[8,10],[15,18]]`

Output: `[[1,6],[8,10],[15,18]]`

Explanation: Since intervals `[1,3]` and `[2,6]` overlaps, merge them into `[1,6]`.

### Solution [own]
```java
class Solution {
    final Comparator<int[]> arrayComparator = new Comparator<int[]>() {
        @Override
        public int compare(int[] a, int[] b) {
            Integer m = a[0];
            Integer n = b[0];
            return m.compareTo(n);
        }
    };
    
    public int[][] merge(int[][] intervals) {
        if(intervals.length <= 1) return intervals;
        int n = intervals.length, i = 0; 
        List<Integer> indices = new ArrayList<Integer>();
        Arrays.sort(intervals, arrayComparator);
        
        while(i < n) {
            if(i<n-1 && intervals[i+1][0] <= intervals[i][1]) {
                while(i<n-1 && intervals[i+1][0] <= intervals[i][1]) {
                    intervals[++i][0] = intervals[i-1][0];
                    intervals[i][1] = Math.max(intervals[i-1][1], intervals[i][1]);
                }
            }            
            indices.add(i++);
        }
        
        int[][] array = new int[indices.size()][2];
        for(int j=0; j<indices.size(); j++) {
            array[j][0] = intervals[indices.get(j)][0];
            array[j][1] = intervals[indices.get(j)][1];
        }
        return array;
    }
}
```

### Solution 
```java
class Solution {
  private class IntervalComparator implements Comparator<int[]> {
    @Override
    public int compare(int[] a, int[] b) {
      return a[0] < b[0] ? -1 : a[0] == b[0] ? 0 : 1;
    }
  }

  public int[][] merge(int[][] intervals) {
    Collections.sort(Arrays.asList(intervals), new IntervalComparator());

    LinkedList<int[]> merged = new LinkedList<>();
    for (int[] interval : intervals) {
      // if the list of merged intervals is empty or if the current
      // interval does not overlap with the previous, simply append it.
      if (merged.isEmpty() || merged.getLast()[1] < interval[0]) {
        merged.add(interval);
      }
      // otherwise, there is overlap, so we merge the current and previous
      // intervals.
      else {
        merged.getLast()[1] = Math.max(merged.getLast()[1], interval[1]);
      }
    }

    return merged.toArray(new int[merged.size()][]);
  }
}
```

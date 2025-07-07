LC1353. Maximum Number of Events That Can Be Attended
=====================================================

# Intuition
The problem requires maximizing the number of events attended, where an event can be attended on any day between its start and end dates. The key insight is to prioritize events that end earlier, as this provides more flexibility for attending other events.

# Approach
The greedy approach works as follows:
1. Find the final day by identifying the maximum end date among all events.
2. Sort events by their start dates to process them chronologically.
3. For each day from 0 to the final day:
   - Add all events that start on or before the current day to a min-heap (priority queue).
   - Remove events from the heap that have already ended.
   - If possible, attend the event with the earliest end time by popping from the heap.
   - This ensures we always choose the event that ends earliest, maximizing future event attendance.

Key steps in the algorithm:
- Use a min-heap to quickly access the event with the earliest end time.
- Dynamically add events as we iterate through days.
- Remove events that are no longer valid (ended before current day).
- Greedily select the event with the earliest end time for the current day.

# Complexity
- Time complexity: $$O(n \log n)$$
  - Sorting events: $$O(n \log n)$$
  - Iterating through days: $$O(finalDay)$$
  - Heap operations: $$O(\log n)$$ per operation
  - Overall: $$O(n \log n + finalDay \log n)$$

- Space complexity: $$O(n)$$
  - Priority queue can store at most $$n$$ events
  - Sorting requires additional space

# Code
```C++ []
class Solution {
public:
    int maxEvents(vector<vector<int>>& events) {
        int finalDay = 0;
        int n = events.size();
        int ans = 0;
        priority_queue<int, vector<int>, greater<>> pq;

        for (int i=0; i<n; i++)
            if (events[i][1] > finalDay)
                finalDay = events[i][1];

        sort(events.begin(), events.end(), [](vector<int> e1, vector<int> e2) -> bool {
            return e1[0] < e2[0];
        });

        int event_idx = 0;
        
        for (int i=0; i<=finalDay; i++) {
            while (event_idx < n && events[event_idx][0] <= i) {
                pq.emplace(events[event_idx][1]);
                event_idx++;
            }

            while (!pq.empty() && pq.top() < i) pq.pop();

            if (!pq.empty()) {
                pq.pop();
                ans++;
            }
        }

        return ans;
    }
};
```

# Key Observations
- The greedy choice of always selecting the event with the earliest end time is optimal.
- This approach ensures maximum flexibility for attending future events.
- The algorithm handles overlapping events by prioritizing events that end earlier.

The code effectively implements this strategy by:
1. Sorting events by start time
2. Using a min-heap to track potential events
3. Iterating through days and selecting the best event to attend
4. Maximizing the number of events attended

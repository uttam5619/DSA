## leetcode 239
### Sliding Window Maximum
`You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.`

```
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {

        int N=nums.size();
        vector<int>result;

        deque<int>deq;

        for(int i=0; i<k-1; i++){
            while(!deq.empty() && nums[deq.back()]<nums[i]){
                deq.pop_back();
            }
            deq.push_back(i);
        }

        for(int i=k-1; i<N; i++){

            while(!deq.empty() && nums[deq.back()]<nums[i]){
                deq.pop_back();
            }
            deq.push_back(i);

            result.push_back(nums[deq.front()]);

            while(!deq.empty() && deq.front()<=i-k+1){
                deq.pop_front();
            }
        }

        return result;
    }
```

`To solve this problem we will use the monotonic deque. The code of monotonic deque will always contain the "while"
condition instead of "if" condition, because if we are using the "if" condition to deal with monotonicity then if the
first element of the window is the largest in the window , and because of it we are not allowing any other element smaller than it to let in the deque, then during the poping out for the very next upcoming window, the front elment
will get pop out and we will have nothing in the deque if the upcoming element is smaller than all the other elements existing in the window.  
`

## leetcode 3261
### Count the substring that satisfy the K constrain II

`You are given a binary string s and an integer k.

You are also given a 2D integer array queries, where queries[i] = [li, ri].

A binary string satisfies the k-constraint if either of the following conditions holds:

The number of 0's in the string is at most k.
The number of 1's in the string is at most k.
Return an integer array answer, where answer[i] is the number of substrings of s[li..ri] that satisfy the k-constraint.`

```
vector<long long> countKConstraintSubstrings(string s, int k, vector<vector<int>>& queries) {       

        int M=queries.size();
        vector<long long>zeroes(M,0);
        vector<long long>ones(M,0);
        vector<long long> both(M, 0);
        vector<long long>result(M,0);

        for(int i = 0; i < M; i++){
            int leftEnd = queries[i][0];
            int rightEnd = queries[i][1];

            int head = leftEnd - 1;
            int tail = leftEnd;

            int one = 0;
            long long count = 0;

            while(tail <= rightEnd){
                while(head + 1 <= rightEnd && (one + (s[head+1] == '1') <= k)){
                    head++;
                    if(s[head] == '1') one++;
                }

                count += (head - tail + 1);

                if(head >= tail){
                    if(s[tail] == '1') one--;
                    tail++;
                } else {
                    head = tail;
                    tail++;
                }
            }

            ones[i] = count;
        }

        // Count substrings with <= k zeroes
        for(int i = 0; i < M; i++){
            int leftEnd = queries[i][0];
            int rightEnd = queries[i][1];

            int head = leftEnd - 1;
            int tail = leftEnd;

            int zero = 0;
            long long count = 0;

            while(tail <= rightEnd){
                while(head + 1 <= rightEnd && (zero + (s[head+1] == '0') <= k)){
                    head++;
                    if(s[head] == '0') zero++;
                }

                count += (head - tail + 1);

                if(head >= tail){
                    if(s[tail] == '0') zero--;
                    tail++;
                } else {
                    head = tail;
                    tail++;
                }
            }

            zeroes[i] = count;
        }

        // Count substrings where BOTH zero <= k AND one <= k
        for(int i = 0; i < M; i++){
            int leftEnd = queries[i][0];
            int rightEnd = queries[i][1];

            int head = leftEnd - 1;
            int tail = leftEnd;

            int zero = 0, one = 0;
            long long count = 0;

            while(tail <= rightEnd){
                while(head + 1 <= rightEnd &&
                      (one + (s[head+1] == '1') <= k) &&
                      (zero + (s[head+1] == '0') <= k)){

                    head++;
                    if(s[head] == '1') one++;
                    else zero++;
                }

                count += (head - tail + 1);

                if(head >= tail){
                    if(s[tail] == '0') zero--;
                    else one--;
                    tail++;
                } else {
                    head = tail;
                    tail++;
                }
            }

            both[i] = count;
        }

        // Final answer using inclusion-exclusion
        for(int i = 0; i < M; i++){
            result[i] = ones[i] + zeroes[i] - both[i];
        }

        return result;
    }
```

This approach will give the TLE. To deal with the TLE we will optimize it.
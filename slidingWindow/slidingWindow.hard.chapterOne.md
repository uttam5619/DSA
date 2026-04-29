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
first element of the window is the largest in the window , and because of the we are not allowing any element to let
in the deque, then during the poping out for the next window, the front elment will get pop out and we will have nothing in the deque if the upcoming element is smaller than all the other elements existing in the window  
`
1. Given an array of integers nums, return the number of good pairs.
A pair (i, j) is called good if nums[i] == nums[j] and i < j.

leetcode 1512

- int numIdenticalPairs(vector<int>& nums) {
        
        unordered_map<int,int>occurances;
        int totalGoodPairs=0;

        for(int i=0; i<nums.size(); i++){
            occurances[nums[i]]++;
        }

        for(auto it:occurances){
            int freq=it.second;
            totalGoodPairs+= ((freq-1)*freq)/2;
        }
        return totalGoodPairs;
    }

// since we are traversing linearly in the array so automatically for the i and j , we have j is lesser thani because is representing the current element. 
The `occurancees[nums[i]]` shows the number of times the current element has occured before the current element.

2. 
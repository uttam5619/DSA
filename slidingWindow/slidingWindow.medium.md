## (Algo Zenith) Number of Subarray with sum at most K 
### Given an array of N integers, find the number of subarrays with a sum less than or equal to K.

```
#include <bits/stdc++.h>
using namespace std;

int main(){

    long long T,N,K,A;
    cin>>T;

    while(T--){
        cin>>N;
        cin>>K;
        vector<int>nums;

        for(int i=0; i<N; i++){
            cin>>A;
            nums.push_back(A);
        }

        int head=-1;
        int tail=0;
        long long count=0;
        long long sum=0;

        while(tail<N){
            while(head+1<N && sum<=K){
                head+=1;
                sum+=nums[head];
            }
            
            //counting all the subarray which has sum>=K.
            if(sum>K){
                count+=(N-head);
            }

            if(head>=tail){
                sum=sum-nums[tail];
                tail++;
            }else{
                head=tail;
                tail++;
            }

        }
        // total subArrays - subArrays with sum>=K. 
        long long result= N*(N+1)/2 -count;
        cout<<result<<endl;
    }

}
```
`"count+=(N-head)".This thing is the most important insight from the question, because if we count the subArray 
as count+=1 then it will count only one sub array that is starting from index "tail" and ending at index "head", this
will lead to the discarding of all the subarrays which has starrting index as "tail" and ending index greater than "head", because any subarry with starting index as "tail", and ending index as some index greater than "head" for the given subarray satisfying the condition, will always have the sum > K.`


## leetcode 2134
### Minimum swap to group all 1's together.

`A swap is defined as taking two distinct positions in an array and swapping the values in them.A circular array is defined as an array where we consider the first element and the last element to be adjacent. Given a binary circular array nums, return the minimum number of swaps required to group all 1's present in the array together at any location.`

```
int minSwaps(vector<int>& nums) {
        int N=nums.size();
        int countOne=0;

        int totalOnes=0;
        for(int i=0; i<N; i++){
            if(nums[i]==1)totalOnes++;
        }

        if(totalOnes==0)return 0;

        int windowSize=totalOnes;
        for(int i=0; i<windowSize-1; i++){
            if(nums[i]==1){
                countOne++;
            }
        }

        int windowWithMaxOne=0;
        vector<int> temp = nums;
        nums.insert(nums.end(), temp.begin(), temp.end());

        for(int i=windowSize-1; i<nums.size(); i++){
            if(nums[i]==1)countOne++;

            windowWithMaxOne=max(windowWithMaxOne,countOne);

            if(nums[i-windowSize+1]==1)countOne--;
        }

        return windowSize-windowWithMaxOne;
    }
```
`First we will find how many 1's are there, so that we can have our window size. Then we will find the window with maximum number of 1's. Suppose the window with maximum number of 1's has 'm' 1's and 'n' 0's, then in such case we need to make n swaps to group all 1 together `

## leetcode 3254
### Find the power of K size subArray

```
vector<int> resultsArray(vector<int>& nums, int k) {

        int N=nums.size();
        if(N==1)return {nums[0]};

        vector<int>result(N-k+1,-1);

        for(int i=0; i<N-k+1; i++){
            int current=i;
            while(current+1<i+k && nums[current+1]-nums[current]==1){
                current+=1;
            }

            if(current==i+k-1){
                result[i]=nums[current];
            }
        }

        return result;
    }
```
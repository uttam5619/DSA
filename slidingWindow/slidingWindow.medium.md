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
will lead to the discarding of all the subarrays which has starrting index as "tail" and ending greater than "head", because any subarry with starting index as "tail", and ending index as some index greater than "head", will always have the sum > K.`
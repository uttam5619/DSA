## 1 `Find the first non-repeating element in a given array arr of integers and if there is not present any non-repeating element then return 0.`
```
int firstNonRepeating(vector<int>& arr) {
        
        int N=arr.size();
        vector<int>freqArray;
        
        unordered_map<int,int>mp;
        for(int i=0; i<N; i++){
            mp[arr[i]]++;
        }
        
        for(int i=0; i<N; i++){
            if(mp[arr[i]]==1){
                return arr[i];
            }
        }
        
        return 0;
}
```

## 2. `Given an array of integers arr[], sort the array according to the frequency of elements, i.e. elements that have higher frequency comes first. If the frequencies of two elements are the same, then the smaller number comes first.`
```
vector<int> sortByFreq(vector<int>& arr) {
        
        int N=arr.size();
        unordered_map<int,int>mp;
        for(int i=0; i<N; i++){
            mp[arr[i]]++;
        }
        
        vector<pair<int,int>>occurances;
        for(auto it:mp){
            int num = it.first;
            int freq = it.second;
            occurances.push_back({num,freq});
        }
        
        sort(occurances.begin(),occurances.end(),[](pair<int,int>a,pair<int,int>b){
            if(a.second==b.second){
                return a.first<b.first;
            }
            return a.second>b.second;
        });
        
        vector<int>result;
        
        for(int i=0; i<occurances.size(); i++){
            int num = occurances[i].first;
            int freq = occurances[i].second;
            while(freq--){
                result.push_back(num);
            }
        }
        
        return result;
        
}
```

## 3. `Given an array arr[], find the first repeating element index. The element should occur more than once and the index of its first occurrence should be the smallest.`

`Note:- The position you return should be according to 1-based indexing`. 
Example:
Input: arr[] = [1, 5, 3, 4, 3, 5, 6]
Output: 2
Explanation: 5 appears twice and its first appearance is at index 2 which is less than 3 whose first the occurring index is 3.
```
int firstRepeated(vector<int> &arr) {
        int N=arr.size();
        unordered_map<int,int>freqMap;
        unordered_map<int,int>firstOccurance;
        
        int minIndex=INT_MAX;
        
        for(int i=0; i<N; i++){
            freqMap[arr[i]]++;
        }
        
        for(int i=0; i<N; i++){
            if(firstOccurance.find(arr[i])!=firstOccurance.end()){
               continue; 
            }else{
                firstOccurance[arr[i]]=i;
            }
        }
        
        for(auto it:freqMap){
            int num=it.first;
            int freq=it.second;
            if(freq>1){
                minIndex=min(minIndex,firstOccurance[num]);
            }
        }
        
    return minIndex==INT_MAX?-1:minIndex+1;
        
}
```
## leetcode 1456.
### Maximum Number of vowels in a subs tring of given length
`Given a string s and an integer k, return the maximum number of vowel letters in any substring of s with length k.`

```
    bool isVowel(char ch){
        if(ch=='a'|| ch=='e' || ch=='i' ||ch=='o' || ch=='u' )
            return true;
        return false;
    }

    int maxVowels(string s, int k) {
        int N=s.size();
        int maxLength=0;
        deque<int>deq;

        for(int i=0; i<k-1; i++){
            if(isVowel(s[i])){
                deq.push_back(i);
            }
        }

        for(int i=k-1; i<N; i++){

            if(isVowel(s[i])){
                deq.push_back(i);
            }

            maxLength=max(maxLength, (int)deq.size());

            while(!deq.empty() && deq.front()<=i-k+1){
                deq.pop_front();
            }
        }

        return maxLength;
    }
```
`In the deq we are inserting the index, and not the character because,
during the poping out of the elements, which do not belongs to the window
the index helps to find out such elements`


## leetcode 567.
### permutation in the string

`Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise. In other words, return true if one of s1's permutations is the substring of s2.`

```
bool checkInclusion(string s1, string s2) {
        if(s2.size()<s1.size())return false;
        int window=s1.size();

        vector<int>occuranceInWindow(26,0);
        vector<int>occuranceInString(26,0);

        for(size_t i=0; i<s1.size(); i++){
            occuranceInString[s1[i]-'a']++;
        }

        for(size_t i=0; i<window-1; i++){
            occuranceInWindow[s2[i]-'a']++;
        }

        for(size_t i=window-1; i<s2.size(); i++){
            occuranceInWindow[s2[i]-'a']++;
            if(occuranceInWindow==occuranceInString){
                return true;
            }
            occuranceInWindow[s2[i-window+1]-'a']--;
        }
        return false;
    }
```
`We have a frequency array for the given window we will make changes in the frequency array and then compare the frequency array.`

```
bool checkInclusion(string s1, string s2) {
        sort(s1.begin(),s1.end());

        int N=s1.size();
        int M=s2.size();

        if(M<N)return false;

        for(int i=0; i<=M-N; i++){
            string temp=s2.substr(i,N);
            sort(temp.begin(),temp.end());
            if(temp==s1){
                return true;
            }
        }
        return false;
    }
```

`We do not need to sort the compplete s2, to check the equality we need to check that wherether the new arrangement sort(s1.begin(),s1.end()) is equal to the new arrangment of the substring of s2 that is  sort(temp.begin(),temp.end())`


## leetcode 219.
### Contains Duplicate II

`Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.`
```
bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int head=-1;
        int tail=0;
        int N=nums.size();

        unordered_map<int,int>mp; // stores number and its last seen index

        /*
        while(tail<N){
            
            while(head+1<N && head+1-tail<=k){
                head=head+1;
                if(st.find(nums[head])!=st.end()){
                    return true;
                }else{
                    st.insert(nums[head]);
                }
            }

            if(head>=tail){
                st.erase(nums[tail]);
                tail++;
            }else{
                head=tail;
                tail++;
            }
        }
        */

        //more appropriate solution than the previous one commented above.
        for(int i=0; i<nums.size(); i++){
            int currentEle=nums[i];
            if(mp.find(currentEle)!=mp.end()){
                if(abs(i-mp[currentEle])<=k){
                    return true;
                }
            }
            mp[currentEle]=i;
        }

        return false;
    }
```
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
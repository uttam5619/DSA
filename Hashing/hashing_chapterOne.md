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


2. `3160. Find the Number of Distinct Colors Among the Balls`
vector<int> queryResults(int limit, vector<vector<int>>& queries) {

        vector<int>result;
        unordered_map<int,int>ballMap; // ball and its color
        unordered_map<int,int>colorBallMap; //each color signify a unique ball.

        int N= queries.size();

        for(int i=0; i<N; i++){
            int ball=queries[i][0];
            int newColor=queries[i][1];
            
            if(ballMap.find(ball)!=ballMap.end()){
                int oldColor = ballMap[ball];
                if(newColor!=oldColor){
                    if(colorBallMap[oldColor]==1){
                        colorBallMap.erase(oldColor);
                    }else{
                        colorBallMap[oldColor]--;
                    }
                    colorBallMap[newColor]++;
                }else{
                    result.push_back(colorBallMap.size());
                    continue;
                }
            }else{
                colorBallMap[newColor]++;
            }

            ballMap[ball]=newColor;

            result.push_back(colorBallMap.size());
        }

        return result;
}


3. `648. Replace Words`.
string replaceWords(vector<string>& dictionary, string sentence) {
        
        string s;
        /*
        declaringt the dictSet to maintain the
        hash of all the root present in the dictionary
        */
        unordered_set<string>dictSet;

        int N=dictionary.size();
        for(int i=0; i<N; i++){
            dictSet.insert(dictionary[i]);
        }

        /*
        declaring the stringStream to tokenize
        the words in string stream.
        */
        stringstream ss(sentence);
        int notTheFirstWord=0;
        string word;

        /*
        driver code
        */
        while(ss>>word){
            string str;
            bool flag=false;
            for(int i=0; i<word.size(); i++){
                str+=word[i];
                /*
                checking whether the portion of the
                word exists as the root or not.
                */
                if(dictSet.find(str)!=dictSet.end()){
                    flag=true;
                    break;
                }
            }
            
            /*
            adding the token
            to form new sentence.
            */
            if(flag){
                if(notTheFirstWord){
                    s+=" ";
                    s+=str;
                }else{
                    s+=str;
                }
            }else{
                if(notTheFirstWord){
                    s+=" ";
                    s+=word;
                }else{
                    s+=word;
                }
            }

            notTheFirstWord++;
        }

        return s;
}
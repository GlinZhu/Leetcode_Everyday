# Topic 1 Binary Search

This section is to discuss general strategies that I used to slove binary search problems in leetcode, and only tricky/hard leetcode problem ID will be listed under this topic.

The general method for bianry search works as follows

![Binary search](./binary_search.png)

### Summary

* Binary search typically sovles problems with sorted arrays and has O(logn) time complexity
* 


### Type 1 Binary search for rotated arrays
Related Leetcode Problems
- [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
    * method 1: 1) find the minimal value of this array(using binary search), 2) determine where the target is located in the left or right half. 3) regular binary search to find the target. (Best TC is O(logn))
    * method 2: Only one binary search is needed. 
- [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
    * CANNOT use binary search for this problem
    * but the worst case of this problem is 111111011111. 
- [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
- [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

#### Note
As for rotated arrays with duplicated elements, binary search CANNOT be applied to this problem. Thus the worst TC is O(n). 



### Leetcode 475 Heaters

Example 1:
```
Input: [1,2,3],[2]
Output: 1
Explanation: The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.
```
Solution 1
```
class Solution {
public:
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        if(houses.empty())
            return true;
        if(heaters.empty())
            return false;
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        int size=houses.size();
        int left=0;
        int right;
        right=abs(max(houses.back(), heaters.back()) - houses.front());
        //cout<<"right\n"<<right<<endl;
        //binary search
        while(left<right){
            int m=left+(right-left)/2;
            if(CoverAllHouses(houses, heaters, m)){
                right=m;
            }
            else{
                left=m+1;
            }
        }
        return left;
    }
    // binary serach if radius is able to cover all houses
    bool CoverAllHouses(vector<int> &houses, vector<int> &heaters, int radius){
        int left = heaters[0] - radius;
        int right = heaters[0] + radius;
        int heater_idx = 0, house_idx = 0;
        
        while(house_idx < houses.size()) {
            if(houses[house_idx] >= left && houses[house_idx] <= right) {
                ++house_idx;
            }
            else {
                ++heater_idx;
                if(heater_idx == heaters.size())
                    return false;
                left = heaters[heater_idx] - radius;
                right = heaters[heater_idx] + radius;
            }
        }
        return true;
    }
    
};
```
Solution 2:
```
class Solution {
public:
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        sort(heaters.begin(), heaters.end());
        int res = 0;
        for(auto& house : houses){
            int l = 0, r = heaters.size()-1, diff = INT_MAX;
            while(l <= r){
                int mid = l + (r-l)/2;
                diff = min(diff, abs(heaters[mid] - house));
                if( heaters[mid] < house){
                    l = mid + 1;
                }else{
                    r = mid - 1;
                }
            }
            res = max(res, diff);
        }
        return res;
    }
}
```

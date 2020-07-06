# Topic 1 Binary Search

This section is to discuss general strategies that I used to slove binary search problems in leetcode, and only tricky/hard leetcode problems will be listed here. 

The general method for bianry search works as follows

![Binary search](./binary_search.png "Binary search approach")

### Summary

* Binary search typically sovles problems with sorted arrays and has **O(logn)** time complexity
* In which cases Binary search can be used??

### Type 1 Binary search for rotated arrays
Related Leetcode Problems
- [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
    * method 1: 1) find the pivot of this given array(using binary search), but need to consider two cases where the given array is rotated and not rotated. 2) find where the target is located in the left or right half. 3) regular binary search to find the target. (Best TC is O(logn)). Also see more details [here](https://www.jiuzhang.com/problem/search-in-rotated-sorted-array-ii/#tag-highlight-lang-cpp)
    * method 2: Only one binary search is needed. See [here](https://www.cnblogs.com/grandyang/p/4325648.html) for more details about the solution. 

**Method logic:**
1. determine where the target is (left half or right half) by comparing between mid and right element of given array. 
2. if its on the right half/left half, then find if target is within `nums[mid]<target` and `nums[right-1]>=target`, so basically the additioanl condition check is added to this case compared to regular binary search template. 
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left=0, right=nums.size();
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target) return mid;
            if(nums[mid]<=nums[right-1]){ //it considers the case (3,1), so has '=';
                //it also considers if target is equal to the most right element. 
                if(nums[mid]<target&&nums[right-1]>=target) left=mid+1;
                else
                    right=mid;
            }
            else{
                if(nums[mid]>target&&nums[left]<=target) right=mid;
                else
                    left=mid+1;
            }
        }
        return -1;
    }
};
```
- [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
    * CANNOT use binary search for this problem, since it has duplicated in the sorted array `(Ep, 1131)`, so just use brute force to achieve O(n).
    * Can also use the method 1 metioned above to optimize the cases that binary serach can be applied to. Also see more details [here](https://www.jiuzhang.com/problem/search-in-rotated-sorted-array-ii/#tag-highlight-lang-cpp)
    * but the worst case of this problem is **11111111111**. 
    **we can still use binary search by comparing nums[mid] and nums[right]；**
    **nums[mid] > nums[right], pivot must be on the right side of mid;**
    **nums[mid] < nums[right] , pivot must be on the left side of mid;**
    **nums[mid] == nums[right], cann't determine where the pivot is (Ep, [3,3,3,1,3]), it could be on the left or right, so --right;**
    ```cpp
    class Solution {
    public:
        bool search(vector<int>& nums, int target) {
            int n = nums.size(), left = 0, right = n - 1;
            while (left <= right) {
                int mid = (left + right) / 2;
                if (nums[mid] == target) return true;
                if (nums[mid] < nums[right]) {
                    if (nums[mid] < target && nums[right] >= target) left = mid + 1;
                    else right = mid - 1;
                } else if (nums[mid] > nums[right]){
                    if (nums[left] <= target && nums[mid] > target) right = mid - 1;
                    else left = mid + 1;
                } else --right;
            }
            return false;
        }
    };
    ```

- [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
    * similiar method like Search in Rotated Sorted Array
- [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
    * see method above in **bold**
    ```cpp
    class Solution {
    public:
        int findMin(vector<int>& nums) {
            int left=0, right=nums.size()-1;
            //find pivot
            while(left<right){
                int mid=left+(right-left)/2;
                if(nums[mid]>nums[right]){
                    left=mid+1;
                }
                else if(nums[mid]<nums[right]){
                    right=mid;
                }
                else{
                    --right;
                }
                
            }
            return nums[left];
        }
    };
    ```


**Note:**
As for rotated arrays with duplicated elements, binary search CANNOT be applied to this problem. Thus the worst TC is O(n).

### Type 2: Binary Search variants for arrays

- [Find peak element](https://leetcode.com/problems/find-peak-element/)
This is another variant of binary search approach, the locgic is as follows
    * since `nums[i] ≠ nums[i+1]`, we can determine where the peak is by comparing `nums[mid]` and `nums[mid+1]`
    * If nums[mid]>nums[mid+1], then peak might be on the left, so `right=mid`; 
    * If nums[mid]<nums[mid+1], then peak might be on the right, so `left=mid+1`;

My Solution(O(logn))
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int left=0, right=nums.size()-1;
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>nums[mid+1]){
                right=mid;
            }
            else{
                left=mid+1;
            }
        }
        return left;
    }
};
```
- [275. H-Index II](https://leetcode.com/problems/h-index-ii/)
This is another variant of binary search implementations. This problem can be solved in O(log(n)) and O(n)
My solution(log(n))
```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int size=citations.size();
        int left=0, right=citations.size();
        while(left<right){
            int mid=left+(right-left)/2;
            if(mid<citations[size-mid-1]) left=mid+1;
            else right=mid;
        }
        return left;
    }
};
```

### Type 3: Binary Search for Ranges in Arrays




### Leetcode 475 Heaters

Example 1:
```
Input: [1,2,3],[2]
Output: 1
Explanation: The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.
```
Solution 1
```cpp
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
```cpp
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




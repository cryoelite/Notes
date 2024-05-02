- [Simple Array Sum](https://www.hackerrank.com/challenges/simple-array-sum/problem?isFullScreen=true)
  Simple Solution, use std::accumulate to solve it.
- [A very big sum](https://www.hackerrank.com/challenges/a-very-big-sum/problem)
  Same as Simple Array Sum, but just use a normal loop and long long.
- [Compare the triplets](https://www.hackerrank.com/challenges/compare-the-triplets/problem?isFullScreen=true)
  Just simple comparison.
- [Plus Minus](https://www.hackerrank.com/challenges/plus-minus/problem?isFullScreen=true)
  Simple loop, count and ratio.
- [Staricase](https://www.hackerrank.com/challenges/staircase/problem?isFullScreen=true)
  ```cpp
  void compute()
  {
      for (int i{}, spaces{n - 1}, hashes{1}; i < n; ++i, hashes = i + 1, spaces = n - hashes)
      {
        while (spaces-- > 0)
        {
          cout << " ";
        }
        while (hashes-- > 0)
        {
          cout << "#";
        }
        cout << "\n";
      }
    }
  ```
- [Mini max Sum](https://www.hackerrank.com/challenges/mini-max-sum/problem?isFullScreen=true)
  ```cpp
    void compute()
    {
      int total{0};
      int min_val{arr[0]};
      int max_val{arr[0]};
      for (int i{}; i < n; ++i)
      {
        total += arr[i];
        min_val = min(arr[i], min_val);
        max_val = max(arr[i], max_val);
      }
      min_sum = total - max_val;
      max_sum = total - min_val;
    }
  ```
- [Birthday Cake](https://www.hackerrank.com/challenges/birthday-cake-candles/problem?isFullScreen=true)
  ```cpp
    void compute()
    {
      int max_elem{arr[0]};
      for(int i{};i<n;++i){
        if(arr[i] ==  max_elem){
          ++result; 
        }
        else if(arr[i] > max_elem){
          max_elem=arr[i];
          result=1;
        }
  
      }
  
    }
  ```
- [Time Conversion](https://www.hackerrank.com/challenges/time-conversion/problem?isFullScreen=true)
  ```cpp
    void compute()
    {
      int hour{stoi(inp_time.substr(0, 2))};
      if (inp_time[8] == 'A')
      {
        hour %= 12;
        result = (hour / 10 < 1 ? "0" : "") + to_string(hour);
      }
      else
      {
  
        hour += 12;
        hour %= 24;
        result = hour == 0 ? "12" : to_string(hour);
      }
      result += inp_time.substr(2, 6);
    }
  ```
- [Grading](https://www.hackerrank.com/challenges/grading/problem?isFullScreen=true)
  ```cpp
    void compute()
    {
      if (grade < 38)
      {
        result = grade;
      }
      else
      {
        int nm5 = grade + (5 - (grade + 5) % 5);
        result = (nm5 - grade < 3) ? nm5 : grade;
      }
    }
  ```
- [Apples and Oranges](https://www.hackerrank.com/challenges/apple-and-orange/problem?isFullScreen=true)
  ```cpp
    void compute()
    {
      for (int i{}, sum_a{}; i < m; ++i)
      {
        sum_a = a + apples[i];
        if (sum_a >= s && sum_a <= t)
        {
          ++count_apples;
        }
      }
  
      for (int i{}, sum_o{}; i < n; ++i)
      {
        sum_o = b + oranges[i];
        if (sum_o >= s && sum_o <= t)
        {
          ++count_oranges;
        }
      }
    }
  ```
- $$x1 + y* v1 = x2+ y*v2 $$ Because y needs to be the same, it has to be the same number of hops.
  $$y* v1 - y*v2= x2-x1  $$
  $$y \lparen v1 - v2 \rparen = x2-x1  $$
  $$y = \frac{x2-x1}{v1 - v2}  $$
- [Between 2 sets](https://www.hackerrank.com/challenges/between-two-sets/problem)
  ```cpp
      int gcd(int a, int b)
      {
          if (b == 0)
              return a;
          return gcd(b, a % b);
      }
  
      int lcm(int a, int b)
      {
          return abs(a * b) / gcd(a, b);
      }
  
      void compute()
      {
          int a_lcm{a[0]};
          int b_gcd{b[0]};
  
          for (int i{}; i < n; ++i)
          {
              a_lcm = lcm(a_lcm, a[i]);
          }
          for (int i{}; i < m; ++i)
          {
              b_gcd = gcd(b_gcd, b[i]);
          }        
          if(b_gcd%a_lcm != 0) {
              result=0;
              return;
          }
  
          for (int i{a_lcm}; i <= b_gcd; i += a_lcm){
              if(b_gcd%i == 0){
                  ++result;
              }
          }
  
      }
  ```
  We need to find the count of distinct integers, such that for each integer x, all elements of array a must be a factor (divisor) of x. And all elements of array b must be divisible by x, or x should be a factor to all of them. For array a, we can find a number that shares its factors with all elements of a, that would be its [[LCM]], call it p, for array b we can find a number that divides all the numbers, that would be the [[GCD]], call it q. So we have an LCM, this is the smallest number divisible by all elements of a, and GCD, the greatest number that divides all of b. If ``p%q == 0`` (p divides q) then that means, p can be used to divide all the numbers of b, because q is a multiple of p, so we find the numbers between p and q that also divide q, so we check all multiples of p between p and q. 
  For ex.:
  For the array ``a= [2,4]``
  For the array ``b= [16,32, 96]``
  LCM of a = p = 4
  GCD of b = q = 16
  16%4=0, so there does exist atleast 2 integers that are factors of all elements of b, and have all elements of a as their factors.
  16/4 = 4, so there's 4 elements between 4 and 16 that can also be part of our result
  We loop at all multiples of p, i.e., 4, so 4, 8, 12 and 16 and find out 4, 8, 16 satisfy that condition. 
  Answer: 3
- [Breaking Best and Worst Record](https://www.hackerrank.com/challenges/breaking-best-and-worst-records/problem)
  ```cpp
      void compute()
      {
          int max_elem{scores[0]};
          int min_elem{scores[0]};
  
          for (int i{1}, score{scores[1]}; i < n; ++i, score = scores[i])
          {
              if (score > max_elem)
              {
                  max_elem = score;
                  ++max_breaks;
              }
              if (score < min_elem)
              {
                  min_elem = score;
                  ++min_breaks;
              }
          }
      }
  ```
  We simply track the min and max element and compare the current element with both, and increase their counts.
-
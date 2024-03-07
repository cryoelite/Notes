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
-
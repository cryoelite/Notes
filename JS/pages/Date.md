- Quite powerful Date and Time utility, obviously implemented on [[Object]].
- Methods
  ``new Date()``// creates a Date Object with now's date and time. Overrides exist with int milliseconds for time since epoch (Jan 1 1970), time since epoch is aka timestamp. Negative values go behind epoch. Or with a string which can be of format "YYYY-MM-DD" etc. and is parsed automatically.
  Or 
  ``new Date(year, month, date, hours, minutes, seconds, ms)``
  ``.getYear()`` Deprecated, avoid. Use ``.getFullYear()`` instead.
  ``.getMonth()`` and the like methods exist as well.
  ``Date.now()`` gets int value which time since epoch in ms till now.
  ``Date.parse(<str>)``
  
  Similarly, for setting
  * [`setFullYear(year, [month], [date])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setFullYear)
  * [`setMonth(month, [date])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMonth)
  * [`setDate(date)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setDate)
  * [`setHours(hour, [min], [sec], [ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setHours)
  * [`setMinutes(min, [sec], [ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMinutes)
  * [`setSeconds(sec, [ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setSeconds)
  * [`setMilliseconds(ms)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMilliseconds)
  * [`setTime(milliseconds)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setTime) (sets the whole date by milliseconds since 01.01.1970 UTC)
  
  All methods have their get/setUTC... counterparts for getting/setting UTC time.
- Date Objects autocorrect theirselves.
  For ex.:
  ```js
  let date = new Date(2013, 0, 32); // 32 Jan 2013 ?!?
  console.log(date); // ...is 1st Feb 2013!
  ```
- Addition [[Operator]] on Date and Int sends a date ahead.
  ```js
  let date = new Date(2016, 1, 28);
  date.setDate(date.getDate() + 2);
  
  console.log( date ); // 1 Mar 2016
  ```
-
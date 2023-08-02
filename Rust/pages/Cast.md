- Rust doesn't implicitly cast any [[Data Type]]s.
  
  Explicit type conversion can be done with [[as]].
  For ex.:
  ```rust
  // Suppress all warnings from casts which overflow.
  #![allow(overflowing_literals)]
  
  fn main() {
      let decimal = 65.4321_f32;
  
      // Error! No implicit conversion
      let integer: u8 = decimal;
      // FIXME ^ Comment out this line
  
      // Explicit conversion
      let integer = decimal as u8;
      let character = integer as char;
  
      // The results might overflow and
      // return **unsound values**. Use these methods wisely:
       unsafe {
          // 300.0 as u8 is 44
          println!(" 300.0 as u8 is : {}", 300.0_f32.to_int_unchecked::<u8>());
          // -100.0 as u8 is 156
          println!("-100.0 as u8 is : {}", (-100.0_f32).to_int_unchecked::<u8>());
          // nan as u8 is 0
          println!("   nan as u8 is : {}", f32::NAN.to_int_unchecked::<u8>());
      }
  }
  ```
-
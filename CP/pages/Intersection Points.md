- Suppose we have orthogonal line segments like so
  ![image.png](../assets/image_1684566689683_0.png)
  
  As we can see, there are 3 intersection points.
- There are 2 types of line intersection algs, orthogonal and general case ([Bowdoin.edu]( https://tildesites.bowdoin.edu/~ltoma/teaching/cs3250-CompGeom/spring17/Lectures/orthosegmintersect-6up.pdf)). The former is quite simpler as we can simply 'sweep' from left to right (Leftmost segment X axis)or top to bottom (Topmost segment Y axis) and count number of intersections.
- In orthogonal line segment intersection, we focus on 3 events, 
  1. Horizontal LS begins
  2. Horizontal LS ends
  3. Vertical LS occurs. 
  The logic for this sweep line is simple, we start from the leftmost X axis point and then go rightwards  processing all 3 events on the X axis. 
  The starting point itself could belong to either a VLS or an HLS, now if we face event 1, we store the Y co-ordinate of the HLS in a data structure like a #TODO , if we face event 2 we search for the Y co-ordinate and remove it from our data structure and if we face event 3 we check for all the Y co-ordinates stored in our data structure within the range [VLS' start Y co-ordinate, VLS' end Y co-ordinate.
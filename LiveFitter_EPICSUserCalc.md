## **LiveFitter_EPICSUserCalc.java**

An ImageJ plugin for displaying a live fit in a profile plot. Includes userCalc functionality, streaming parameter values for futher calculation and/or record keeping. To use, choose fit function, color of the line, a secondary fill color, the line width, symbol, plot label if desired, and visibility (only uncheck if you don't want to have the fit line). Then draw a line or rectangle, and select draw fit.
![livefittingplugin](https://user-images.githubusercontent.com/106117997/198303604-a3a6a4d1-68a5-455c-b956-b14bbb21937c.png)

If it gives you an error: "Line or rectangular selection required" you may have selected another image window in between drawing the line cut and trying to display the fit. You can change fit settings in between drawing the line cut and displaying the fit, but not another image window/plot. Just click on the image you want to fit from and try displaying fit again.

![Lineselectionerror](https://github.com/abrewe/Documentation/assets/106117997/32696719-a53a-439a-a079-2c43da34d84e)

The Fit Parameters and Fit Equation are displayed above the profile plot.
Includes the option to send and display fit parameters into a userCalc. 
Fits all built-in ImageJ functions and a custom Slit Function (need to specify positive or negative in order to reduce instances of less useable fits--flipping to positive when a negative fit is better, etc). 

$f(x) = a + bx + e/2 * erf( (x - c + d/2) / (f\*\sqrt{2}) ) – e/2 * erf( (x - c - d/2) / (f\*\sqrt{2}) )$

![slitfunctionex](https://user-images.githubusercontent.com/106117997/198300713-61eb3639-a41c-4f4e-a6e8-c7fc322e5d4e.png)


a+b*x : linear background (b limited between -0.05 and 0.05 to get useable results)
c: center of slit function
d: Full width at half max of slit
e: full height of the slit function
f (sigma): width of the erf, blurriness
of the edge (f limited from 0-30 to get better results from custom ImageJ fitting)

userCalc functionality: To send the fit parameters to a userCalc, enter the userCalc prefix 
in the userCalc field (don't put an entire pv name in, just the prefix - eg. 1bmc:userCalc7, not 1bmc-userCalc7.A)

![userCalcEx](https://user-images.githubusercontent.com/106117997/198303770-b0eb0eb4-1d81-42ad-9206-120b432f4d8c.png) 

Displays equation (as much as will fit) and fit parameters in the userCalc. 

![userCalcdisplay](https://user-images.githubusercontent.com/106117997/198303930-042c4d0c-6e64-4d2e-a4c6-2285a120eeb1.png)

Limitations:
- Only works with one fit at a time
- Longer equations may not be fully displayed in userCalc
- Limitations on f parameter for slit function fitting means there may be cases where fit is not great

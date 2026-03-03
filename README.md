# Exporoll
A tonemapper DaVinci Resolve DCTL that can be used to create a pleasing rolloff curve for linear or HDR picture while preserving linear contrast in shadows and midtones.  It combines a blend between linear and asymptotic "Preroll", with a filmic style rolloff.  Internally, it applies the function and gamma to a negative picture which is then uninverted for classic, film-style look.


-----------------------------------------------
MAIN CONTROLS:

**Global_Exp-/+ Stops** - exposure, measured in n stops, or gain = 2^n.  Applied before any rolloff operation, staticpoint, or thresholding.

**Flash** - Equivalent to DaVinci's 'offset'.  Adds or subtracts some value across all pixels.  The most apparent effect to human vision is that it raises shadows and blackpoint, similarly to "flashing" film or spiking a lens.  Applied before any rolloff operation, staticpoint, or thresholding.

**Staticpoint**- a point that remains in place during corresponing operation.  Note that it is applied before gamma, so is displaced by gamma adjustments

**Threshold controls** specify an input value below which the original (with exp/flash corrections applied) linear signal is used, and how much the result is blended over the normal rolloff result.  Note that for now it is a hard threshold with no automatic tangent continuity with rolloff curve.

**Ease_Preroll_Strength_Ctrl** makes preroll strength (normally a lerp by default) follow an easing curve for more fine control over whitepoint.  

**Test Gradient** throws curve graph onto waveform by mapping a linear ramp from black to white on the x axis.  In waveform monitor: x axis is inputvalue, y axis is output value.
**Test Gradient White lvl** is the maximum value of the test gradient.

-----------------------------------------------
ROLL TYPES AND ORDER:

**Preroll (or Rotlog)**- linear (or eased) interpolation between original (with exp/flash corrections applied) linear signal and an inverse log function applied to negative picture which is then inverted again to a positive that asymptotically approaches 1.0.  Input gain adjusted to satisfy staticpoint.  Referred to also as 'Rotlog', or 'rotated logarithmic' since the function essentially rotates and flips the graphed signal of a typical log transform.  Instead of becoming more linear as it gets brighter, it curves more as it gets brighter, keeping shadows and midtones closer to linear values.  Gamma is applied last.

**Main Exporoll**- Accepts blend between linear (preroll = 0) and preroll of some strength (preroll > 0).  Pulldown then gamma, or exponent, applied to negative picture, then inverted again for positive output.  Input gain adjusted to satisfy staticpoint.  'Shoulder offset' value is added to exponent independent of staticpoint.  Gamma is applied last

**Log controls** make a log curve, of course.  Note that this is also affected by preroll.  

---------------------------------------------
EXPERIMENTAL FEATURES:

These are features that only sort of work and may not continue to subsequent versions.

  **Hybrid Mode**- If the experimental **Hybrid Mode** is enabled, the log-like curve resulting from preroll and log controls can be blended with the Main roll using the 'Blend_log_into_Rolloff' slider.  Note that preroll affects both sides of the blend and the result can be kind of ugly.  Like I said, experimental.

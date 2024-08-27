# citation

# summary

# notes
predict probabilistic distribution of motion blue at the patch level using a CNN
- Expand dataset by carefully designed image rotations (extend the motion kernel set of the CNN)
- Markov random field model is used to infer a dense non-uniform motion blur field  enforcing motion smoothness
- Motion blur is removed by a non-uniform deblurring model (at the patch level, not uniform across the image)
- they first try to determine the direction of motion (motion vector)
- Use a CNN to deblur

Not super relevant to our work, biggest takeaway is to avoid uniform deblurring; it is better to go patch by patch.

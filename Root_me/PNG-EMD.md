# Description
<img width="1011" height="403" alt="image" src="https://github.com/user-attachments/assets/de3e7425-09d6-4975-ac68-a399a537d328" />

# Solve
We were given a black-and-while image
<img width="960" height="607" alt="ch26" src="https://github.com/user-attachments/assets/afd5010e-2086-449d-b549-05d9d68822cd" />

Based on challenge title and the information, we guess that they use `EMD` algorithm to hide the secret message.

The secret message will be converted to $(2n + 1)-ary$, then each of secret digits are called $d$. So let's explore that algorithm first. Looking at this formula:

$f_e(g_1, g_2,...,g_n) = (g_1 * 1 + g_2 * 2 + ... + g_n * n) mod (2n + 1)$

Where:

* $g$: Pixel value.
* $(g_1, g_2,...,g_n)$: Pixel group.
* $n$: number of pixel in a group.

 After calculating to $f_e$, we need to calculate the $r$ value by using:

 $r = (f_e - d) mod (2n + 1)$

Based on $r$ value we have 3 ways:
* If $1<=r<=n$, then $g_r$ is increased by 1.
* If $r>=n$, then $g_{2n+1-r}$ decreased by 1.
* If $r=n$, do nothing.

There is the formula using to embed the messega into 
















With this type image, we have 8-bits for a pixel

# Description
<img width="1011" height="403" alt="image" src="https://github.com/user-attachments/assets/de3e7425-09d6-4975-ac68-a399a537d328" />

# Solve
We were given a black-and-while image
<img width="960" height="607" alt="ch26" src="https://github.com/user-attachments/assets/afd5010e-2086-449d-b549-05d9d68822cd" />

Based on challenge title and the information, we guess that they use `EMD` algorithm to hide the secret message.

So let's explore that algorithm first. Look at that formula:

$f_e(g_1, g_2,...,g_n) = (g_1 * 1 + g_2 * 2 + ... + g_n * n) mod (2n + 1) $

Where:

* $g$: pixel value
* $n$: pixer / group




There is the formula using to embed the messega into 
















With this type image, we have 8-bits for a pixel

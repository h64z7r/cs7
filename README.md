java c
SciComp Project 1
Weeks 1+2
1 Background
Many of you will know the phenomenon that a prism refracts light, i.e. splits it up in different colors, because the refractive index of the prism varies with the frequency ω of light. The underlying property of the molecules forming the material is the frequency dependent polarizability, α(ω). The polarizability, like many other properties of molecules and materials, can be calculated from the basic laws of physics, here the the time-dependent Schr¨odinger equation.

Figure 1: A dispersive prism slows light at different rates depending on the wave-length, causing refraction.
In the end, the polarizability for a given frequency ω of the incoming light is obtained as the following scalar product of two column vectors z and x:
α(ω) = zTx                                                                       (1)
where z is a vector that can be calculated from the Schr¨odinger equation, and x is the solution to the following system of linear equations:
(E − ωS)x = z                                                                   (2)
Here, E and S are two square matrices, and ω is the frequency of the incoming light. Like the column vector z, the matrices E and S are calculated from the Schr¨odinger equation, which we will not discuss further in this course.
2 Data
In this project we consider the water molecule, H2O, and its frequency dependent polarizability, α(ω). It turns out that the matrices E and S, and the column vector z, have a structure that lets us decompose the matrices into submatrices as follows:

The Python-file watermatrices.py contains the submatrices A and B, and the subvector y for a water molecule, obtained by an approximate solution to the Schr¨odinger equation. From this, you can construct the full matrices E and S, which in this approximation should be 14 × 14.
3 Questions for Week 1
a. (1) Write a small function that computes the condition number of a matrix under the max-norm:
cond∞(M) = ||M||∞ ||M−1||∞
Use a library matrix inversion routine for M−1, but do program the max-norm yourself using sum, abs, and max. (2) For three frequencies, ω = {0.800, 1.146, 1.400}, calculate the condition number for the matrix E − ωS. The right-hand-side z is given with 8 significant digits. How many significant digits could we guarantee in the solution x if everything else were assumed exact? Why?
b. (1) For each of the three ω, determine a bound on the relative forward error in the max-norm:

for the perturbation that the frequency ω is changed by δω = 2/1 · 10−3. Recall that ˆx is the computed approximate value (known) of the exact x (unknown), and ∆x ≡ x − ˆx. (2) As ω is given with 3 digits after the comma, how many significant digits could we be guarantee in x if everything else were exact? Why?
c. Implement three separate functions
1. L,U = lu factorize(M), which takes a square matrix M as input and returns two square matrices: A triangular matrix L and upper triangular matrix U such that M = LU.
2. y = forward substitute(L,z), which takes a square lower triangular matrix L and a vector b as input, and returns the solution vector y to Ly = b.
3. x = back substitute(U,y), which takes a square upper triangular matrix U and a vector y as input, and returns the solution vector x to Ux = y.
and test them with the linear equation

You can test your solution against a library routine.
If you know how, try to use vector operations instead of for-loops where possible (orders of magnitude faster in Python).
d. Implement a function α = solve alpha(omega) for calculating the frequency-dependent polarizability α(ω) = zTx for water in the given approximation. This routine should solve Equation (2) by LU-factorization using your own three routines from (c). (1) Using your ro代 写SciComp Project 1Python
代做程序编程语言utine, make a table of the polarizabilities for the frequencies given in (a) and their pertur-bations, i.e. for ω = {0.800 ± δω, 1.146 ± δω, 1.400 ± δω}, with δω = 2/1 · 10−3 as before. (2) Which error-bound is the correct one to understand the variation of the calculated polariz-abilities due to the perturbation: (a) or (b) or both? Explain why. (3) Use this bound to derive an upper bound for ∆α(ω) = α(ω + δω) − α(ω) of the form.|∆α(ω)| ≤ B(ω)|δω|. Do your calculated values fall within this bound?
e. (1) Compute a table of α(ω) for 1000 evenly spaced values in the interval [0.7, 1.5] using your routine from (d), and plot the values. (2) Can you explain what happens to the linear system of Equation (2) around the frequency ω = 1.146307999, and how is this reflected in α(ω)?
4 Questions for Week 2
f. Implement and test the Householder and least-squares routines below.
1. Q,R = householder QR slow(A), which takes as input a rectangular matrix M: m × n and uses the Householder method to compute its QR decomposition. Check that Q: m × m is orthogonal, i.e., QT Q = QQT = I, and that the upper triangular matrix R: m × n satisfies M = QR.
2. A more efficient version of (f.1) that doesn’t compute the m×m matrix Q, but just stores the reflection vectors v. You can either give it the interface V,R = householder fast(A) or (as is done in practice) let the result be a combined (m + 1) × n matrix VR, in which the upper n × n triangle contains R, and the kth column below the diagonal is vk (of length m − k).
3. ˜x, r = least squares(A,b), which combines this routine with your back-substitution from (c.3) to compute a linear least squares fitting. It should take as input a rectangular m×n matrix A and an m×1 right-hand-side vector b, returning an n×1 approximate solution vector ˜x to A ˜x ' b as output, and the residual. If you did not get (f.2) to work, you can just use your solution to (f.1).
You can use the linear system in Heath’s Example 3.1  3.8 to debug individual steps of your computation. Test your routines against A1, b1 from HHexamples.py, and print the resulting upper triangular R and solution x rounded to 3 digit accuracy.
g. We now want to approximate α(ω) in the interval [0.7, ωp] using a polynomial

(1) Suggest a suitable value of ωp < 1.5. What makes this choice reasonable? (2) Find the coefficients of the polynomial using your linear least squares routine from (f), the table computed above, and n = 4. (3) Repeat the computation for n = 6 and compare the accuracies of the two polynomials: Plot the magnitude of the relative error (in a log10-scale) of the polynomial approximation as the difference between the P(ω) and α(ω) values. (4) How many significant digits does each approximation yield?
h. We would now like to approximate α(ω) in the entire interval [0.7, 1.5], and choose the rational approximating function, which is able to represent singularities:

(1) Why will this fail with the polynomial approximation of Problem (g)? And why can Eq. (5) do a better job? (2) Find the coefficients aj and bj using your linear least squares routine, the table of α-values computed above, and n = 2. You need to reformulate the expression as an linear approximation so that you can use a linear least squares fitting. Plot the error of the the rational-function approximation Q(ω) compared to α(ω) calculated by Equations (1) and (2). (2) Repeat the computation for n = 4 and compare the accuracies of the two approximations quantitatively. (3) If you look at α(ω) in the extended interval ω ∈ [−4; 4], you will notice that α(ω) has multiple singularities. Are you able to modify your approximation to accurately approximate the full interval [−4; 4], in particular so that it reproduces the singularities correctly? Explain your solution.


         
加QQ：99515681  WX：codinghelp

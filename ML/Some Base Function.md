# Polynomial
$$\phi_j(x) = x^j$$
$$\Phi: \mathbb{R} \to \mathbb{R}^d$$
$$x \mapsto (1,x,x^2,...,x^d)$$

# Gaussian
$$\phi_j(x) = \exp{\left( -\frac{1}{2}\left( \frac{x - \mu}{\sigma} \right)^2\right)}$$

# Sigmoid
$$\phi_j(x) = \sigma{\left(\frac{x-\mu_j}{s}\right)} = \frac{1}{1+e^{-\tfrac{x-\mu_j}{s}}}$$
$$\Phi: \mathcal{X} \to \mathbb{R}^d$$
$$x \mapsto \begin{pmatrix}
\sigma{((x-\mu_1)/s)}\\
\sigma{((x-\mu_2)/s)}\\
\vdots\\
\sigma{((x-\mu_m)/s)}
\end{pmatrix}$$
# Hyperbolic tangent
$$\phi_j(x) = \tanh{\left(\frac{1}{2} \cdot \frac{x-\mu_j}{s}\right)} = 2\sigma{\left(\frac{x-\mu_j}{s}\right)} - 1 = \frac{1 - e^{-\dfrac{x-\mu_j}{s}}}{1+e^{-\dfrac{x-\mu_j}{s}}}$$
$$\Phi: \mathcal{X} \to \mathbb{R}^d$$
$$x \mapsto \begin{pmatrix}
2\sigma{((x-\mu_1)/s)} - 1\\
2\sigma{((x-\mu_2)/s)} - 1\\
\vdots\\
2\sigma{((x-\mu_m)/s)} - 1
\end{pmatrix}$$
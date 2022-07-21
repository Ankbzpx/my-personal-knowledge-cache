
> Reference: http://sepwww.stanford.edu/data/media/public/docs/sep115/jun1/paper_html/node2.html

Choose $diag(\bm{W}) = |\bm{r}|^{(p-2)}$, so that the loss function
$$
\bm{r}^T \bm{W} \bm{r} = \bm{r}^T |\bm{r}|^{p-2} \bm{r} = \bm{r}^p
$$
which minimize the $l_p$ norm of the residual (usually $1 \le p \le 2$)

## Steps
1. Start with $diag(\bm{W}) = \bm{1}$
2. Compute weighted least square fit and residual $\bm{r}$
3. Adjust weight $diag(\bm{W}) = |\bm{r}|^{(p-2)/2}$
4. Re-fit and repeat for few iterations

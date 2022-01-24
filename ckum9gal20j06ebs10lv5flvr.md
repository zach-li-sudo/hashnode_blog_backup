## My first article! Nothing but a test🍻

 #### Try latex support

Descent methods:
$$
x^{k+1}=x^{k}+\alpha^k d^k
$$

1. \\(d^k=-\nabla f(x^k)\\), steepest descent
2. \\(d^k=-[\nabla^2 f(x^k)]^{-1}\nabla f(x^k)\\), Newton's method
3. \\(d^k\approx-[\nabla^2 f(x^k)]^{-1}\nabla f(x^k)\\)), Quasi-Newton methods (e.g. BFGS)


```
def myNewtonSolver(func, gradient, hessian, x0, maxIter, tol, **kwargs):
    return [x, iter]
```


### Just added Github backup for this post! 👏


![xtrajectory.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642989630825/llE4fVfwI.jpeg)

![error.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642989652679/heMKEPTVr.jpeg)
![func_val.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642989642386/2K9oaxAUu.jpeg)

# George598Project1

We take into consideration a simple formulation of rocket landing where the rocket state  is represented by its distance to the ground  and its velocity , i.e., 
, where  specifies time. The control input of the rocket is its acceleration . The discrete-time dynamics follows:
                                           
                                           d(t+1)= d(t) +v(t)∆t
                                           v(t+1)= v(t) +a(t)∆t

 
where ∆t is a time interval. Further, let the closed-loop controller be

                                           a(t)= f_θ (x(t))

where f_θ(.) is a neural network with parameters θ, which are to be determined through optimization.

For each time step, we assign a loss as a function of the control input and the state:l(x(t),a(t)) . In this example, we will simply set l(x(t),a(t))=0 for all t= 1,....,T-1, where T  is the final time step, and l(x(t),a(t)) =||x(t)||^2 = d(T)^2 + v(T)^2
. This loss encourages the rocket to reach d(T)=0 and v(T)=0 , which are proper landing conditions.

The optimization problem is now formulated as:

                                                    min ||x(T)||^2
 	                                                   θ
                                                     
                                                   s.t.    d(t+1)= d(t) +v(t)∆t
                                                           v(t+1)= v(t) +a(t)∆t 
                                                           a(t)= f_θ (x(t)) ⩝t= 1,……….T-1
 
While this problem is constrained, it is easy to see that the objective function can be expressed as a function of x(T-1) & a(T-1) , where x(T-1) is a function of x(T-2) & a(T-2) and so on. Thus it is essentially an unconstrained problem with respect to θ.

We calculated the gradient of the loss function ∇_θ(l(x(T),a(T)) using LBFGS and ran the code for number of iterations = 40. We ran the code and got convergence on our results after around 23 iterations .


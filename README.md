# George598Project1

Consider a simple formulation of rocket landing where the rocket state  is represented by its distance to the ground  and its velocity , i.e., 
, where  specifies time. The control input of the rocket is its acceleration . The discrete-time dynamics follows:
                                           
                                           d(t+1)= d(t) +v(t)∆t
                                           v(t+1)= v(t) +a(t)∆t

 
where ∆t is a time interval. Further, let the closed-loop controller be

                                           a(t)= f_θ (x(t))

where 
 is a neural network with parameters , which are to be determined through optimization.

For each time step, we assign a loss as a function of the control input and the state: . In this example, we will simply set  for all , where  is the final time step, and 
. This loss encourages the rocket to reach  and , which are proper landing conditions.

The optimization problem is now formulated as

 	
 
While this problem is constrained, it is easy to see that the objective function can be expressed as a function of , where  as a function of  and , and so on. Thus it is essentially an unconstrained problem with respect to .

In the following, we code this problem up with PyTorch, which allows us to only build the forward pass of the loss (i.e., how we move from  to  and all the way to ) and automatically get the gradient 
.

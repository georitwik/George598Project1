# George598Project1

We take into consideration a simple formulation of rocket landing where the rocket state  is represented by its distance to the ground  and its velocity , i.e., 
, where  specifies time. The control input of the rocket is its acceleration . The discrete-time dynamics follows:
                                           
                                           d(t+1)= d(t) +v(t)∆t
                                           v(t+1)= v(t) +a(t)∆t

 
where ∆t is a time interval. Further, let the closed-loop controller be

                                           a(t)= f_θ (X(t)), a(t)=[thrust ,𝜃']
                                                                   

where f_θ'(.) is a neural network with parameters θ, which are to be determined through optimization.

For each time step, we assign a loss as a function of the control input and the state:l(x(t),a(t)) . In this example, we will simply set l(x(t),a(t))=0 for all t= 1,....,T-1, where T  is the final time step, and l(x(t),a(t)) =||x(t)||^2 = d(T)^2 + v(T)^2
. This loss encourages the rocket to reach d(T)=0 and v(T)=0 , which are proper landing conditions.



                          
 
While this problem is constrained, it is easy to see that the objective function can be expressed as a function of x(T-1) & a(T-1) , where x(T-1) is a function of x(T-2) & a(T-2) and so on. Thus it is essentially an unconstrained problem with respect to θ.

Equations for the problem :


            state x = [dx,dy,Vx,Vy,θ]^T
            action a = [α,∆θ]
            Drag   Fd= -1/2.Cd.ρ.A.(speed)^2
   
   
                  where Fd= Drag force on the rocket
                        Cd= Coefficient of drag (=0.75 for a typical rocket during landing)
                        ρ= Density of surrounding air (=1.225 kg/m^3 is density of air at mean sea level)
                        A= Reference area (=6.16 m^2 for PSLV of ISRO)
                        
 The optimization problem is now formulated as: 
        
        
              min ||X(T)||^2
 	             θ
   
             dx(t+1)= dx(t) + Vx(t).∆t-1/2.∆t^2.α.sinθ(t)
             dy(t+1)= dy(t) + Vy(t).∆t+1/2.∆t^2.(α.cosθ(t)-g)
             Vx(t+1)= Vx(t) - ∆t.α.sinθ(t)
             Vy(t+1)= Vx(t) + ∆t.α.sinθ(t)
             θ(t+1)= θ(t) + ∆θ
             

We calculated the gradient of the loss function ∇_θ(l(x(T),a(T)) using LBFGS and ran the code for number of iterations = 



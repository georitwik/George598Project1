# George598Project1
          {
 "cells": [
  {
   "cell_type": "markdown",
   "id": "attempted-merchandise",
   "metadata": {},
   "source": [
    "# Project 1: Gradient-based Algorithms and Differentiable Programming\n",
    "\n",
    "\n",
    "## 1. Introduction\n",
    "Consider a simple formulation of rocket landing where the rocket state $x(t)$ is represented by its distance to the ground $d(t)$ and its velocity $v(t)$, i.e., $x(t) = [d(t), v(t)]^T$, where $t$ specifies time. The control input of the rocket is its acceleration $a(t)$. The discrete-time dynamics follows \n",
    "\n",
    "$$\n",
    "\\begin{aligned}\n",
    "d(t+1) = d(t) + v(t) \\Delta t, \\\\\n",
    "v(t+1) = v(t) + a(t) \\Delta t,\n",
    "\\end{aligned}\n",
    "$$\n",
    "\n",
    "where $\\Delta t$ is a time interval. Further, let the closed-loop controller be \n",
    "\n",
    "$$\n",
    "a(t) = f_{\\theta}(x(t))\n",
    "$$\n",
    "\n",
    "where $f_{\\theta}(\\cdot)$ is a neural network with parameters $\\theta$, which are to be determined through optimization.\n",
    "\n",
    "For each time step, we assign a loss as a function of the control input and the state: $l(x(t),a(t))$. In this example, we will simply set $l(x(t),a(t))=0$ for all $t=1,...,T-1$, where $T$ is the final time step, and $l(x(T),a(T)) = ||x(T)||^2 = d(T)^2 + v(T)^2$. This loss encourages the rocket to reach $d(T)=0$ and $v(T)=0$, which are proper landing conditions.\n",
    "\n",
    "The optimization problem is now formulated as\n",
    "\n",
    "$$\n",
    "\\begin{aligned}\n",
    "\\min_{\\theta} \\quad & ||x(T)||^2 \\\\\n",
    "\\quad & d(t+1) = d(t) + v(t) \\Delta t, \\\\\n",
    "\\quad & v(t+1) = v(t) + a(t) \\Delta t, \\\\\n",
    "\\quad & a(t) = f_{\\theta}(x(t)), ~\\forall t=1,...,T-1\n",
    "\\end{aligned}\n",
    "$$\n",
    "\n",
    "While this problem is constrained, it is easy to see that the objective function can be expressed as a function of $x(T-1) and a(T-1)$, where $x(T-1)$ as a function of $x(T-2)$ and $a(T-2)$, and so on. Thus it is essentially an unconstrained problem with respect to $\\theta$. \n",
    "\n",
    "In the following, we code this problem up with [PyTorch](https://pytorch.org/), which allows us to only build the forward pass of the loss (i.e., how we move from $x(1)$ to $x(2)$ and all the way to $x(T)$) and automatically get the gradient $\\nabla_{\\theta} l(x(T),a(T))$.\n",
    "\n",
    "---\n",
    "\n",
    "## 2. Sample Code\n",
    "\n",
    "Before you start, please make sure you install the PyTorch package in Python. If you are using Pycharm, you can do so through *setting*->*Project: Your Project Name*->*Project Interpreter*->*Install (the little plus sign to the right of the window)*. "
   ]
  },

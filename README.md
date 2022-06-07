# Almost-embarrassingly-parallel-python
Methods for (almost) embarrassingly parallel programming in  Python

## Introduction to the problem

### The problem

Here is the situation - you have written a serial Python code that runs and everything is fine. You have done some reasonable optimizations and cleaning of the code, but there is this problem... You have to run the code on (let's say 500 000) inputs from a single file (Molecular Dynamics trajectory is the real data behind the inspiration of this tutorial). Just to make things worse - you **CANNOT** load the entire set of inputs in to the computer memory to use traditional methods of applying a function on to each element of a list...  
Let's just acknowledge several possible easy approaches, providing that you have somewhat almost [embarrassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) problem to solve like sum or average of some property over the set of inputs.

- Split the file and start multiple instances of the code, collect the partial data, then merge the result at the end.
- If splitting is not trivial, one can adapt the code to only analyze a slice of the data and run multiple instances as before.
- Import your data in some database (sqlite) and pick subsets at will, perhaps add partial results to each entry...

Perhaps there are other but, more or less, they will require to step outside the Python code to merge the result at the end. Sure, one can do this in the same Python code and pollute the good work with unrelated code...

Or use some easy to implement task scheduling method to distribute the work, collect the partial results and have the final result at the end of the code.

### Simple toy problem to introduce the concepts

For simplicity we will generate random aray data with shape (n,m) that will represent a single input set and then calculate the sum on each row to emulate some sort of analysis. This purpose of the code is to be simple to understand, not to be fast to run. There is an easy way to speed this particular problem, but this is not the goal.

```python
# Let's have some simple data.
data= np.random.rand(50,100)

# Simple analysis - calculates the sum in each row of the data.
def analyze(data):
    sums=np.zeros(data.shape[0])
    for i in range(data.shape[0]):
        sum= 0
        for j in range(data.shape[1]):
            sum= sum + data[i,j]
        sums[i]=sum
    return sums
```
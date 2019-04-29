# Tensorflow Custom Mudule



## Gradient tape
This API can be use for automatic differentiation computing.

 - From the input/output point of view, the Gradient Tape can return the differentiation of any two tensors.
 - Not very usefull for me
 - can be used in a model to return  the gradient differentiation.

### Usage
```python
x = tf.ones((2, 2)) # create a new tensor\
with tf.GradientTape() as t: #create a new tape
  	t.watch(x) # ???
  	y = tf.reduce_sum(x) # apply computing  
  	z = tf.multiply(y, y) # apply computing
dz_dx = t.gradient(z, x) ### ma	in function which return the differentiation of z with respect to x
# we can change x to y or any intermediate value, too.

## judge the result
for i in [0, 1]:
  	for j in [0, 1]:
    	assert dz_dx[i][j].numpy() == 8.0
```

### Tips
 - **Released** when GradienTape.gradient() is called. For mutiple gradients, we can use  
```python
	with tf.GradientTape(persisten=True) as t:
		...
```
 - the two **parameters** can be any tensors which have relationship with each other in a graph. So that the process of constructing a graph can contains some "python clauses" like **"if", "for"**. In further, the process can be conducted in a pre-defined function.
```python
def f(x, y):
  output = 1.0
  for i in range(y):
    if i > 1 and i < 5:
      output = tf.multiply(output, x)
  return output
# this is a very nice way to return the tensor of gradient, from the input/output point of view. 
# As the codes were written in a "Eager Execution" section, it is hard to tell if the return VALUE is a tensor (or can be treated as a tensor).
def grad(x, y): 
  with tf.GradientTape() as t:
    t.watch(x)
    out = f(x, y)
  return t.gradient(out, x)
```



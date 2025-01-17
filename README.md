
## About: 

- Real world has lots of sequential data (non-i.i.d data) e.g Time-series: stock market, speech, video analysis
-  If underlying process unknown,  we can construct model to predict next word/event in sequence
- In Natural Language Processing, Sequence Modeling is the task of predicting what word/letter comes next.

 - In sequence learning problems, we know that the true output at timestep ‘t’ is dependent on all the inputs that the model has seen up to the time step ‘t’. Since we don’t know the true relationship, we need to come up with an approximation such that the function would depend on all the previous inputs.

![image](https://user-images.githubusercontent.com/3470924/117558926-7f415a00-b0c4-11eb-801f-4450fd61d718.png)


The key thing to note here is that the task is not changing for every timestep, whether we are predicting the next character or tagging the part of speech of a word. The input to the function is changing at every time step because for longer sentences the function needs to keep track of the larger set of words.
In other words, we need to define a function that has these characteristics:

- Ensure that the output Yt is dependent on previous inputs
- Ensure that the function can deal with a variable number of inputs
- Ensure that the function executed at each time step is the same.

  ![image](https://user-images.githubusercontent.com/3470924/117558932-85373b00-b0c4-11eb-9172-1d9811535342.png)

Recurrent Neural Networks(RNN) are a type of Neural Network where the output from the previous step is fed as input to the current step.

In RNN, you can see that the output of the first time step is fed as input along with the original input to the next time step.
The input to the function is denoted in orange color and represented as an xᵢ. The weights associated with the input is denoted using a vector U and the hidden representation (sᵢ) of the word is computed as a function of the output of the previous time step and current input along with bias. The output of the hidden represented (sᵢ) is given by the following equation,

Once we compute the hidden representation of the input, the final output (yᵢ) from the network is a softmax function of hidden representation and weights associated with it along with the bias. We are able to come up with an approximate function that is able to satisfy all the three conditions that we have set to solve the problems of sequence learning.



----


### Case Studies: 


####  1. Recommender Engine


see blog post: https://www.asjadk.io/music/

#### 2. Decision Support for Context-aware Resource Allocation using LSTM networks: 


The allocation of resources to process tasks can have a significant impact on the performance (such as cost, time) of those tasks, and hence of the overall process. Past resource allocation decisions, when correlated with process execution histories annotated with quality of service (or performance) measures, can be a rich source of knowledge about the best resource allocation decisions. The optimality of resource allocation decisions is not determined by the process instance alone, but also by the context in which these instances are executed. This phenomenon turns out to be even more compelling when the resources in question are human resources. Human workers with same the organizational role and capabilities can have heterogeneous behaviors based on their operational context. In this work, we propose an approach to supporting resource allocation decisions by extracting information about the process context and process performance from past process executions. 

| DataSet                              | Configuration | Epochs | Train   | Valid   | Test    |
| ------------------------------------ | ------------- | ------ | ------- | ------- | ------- |
| ResourceWithAmountContext            | Medium        | 13     | 74.045  | 187.323 | 191.933 |
| ResourceWithOUtContext               | Medium        | 13     | 2.050   | 2.321   | 2.518   |
| ResourceWithWL&AmountContext         | Medium        | 13     | 77.944  | 161.044 | 164.025 |
| ResourceWithAmountFamiliarityContext | Medium        | 13     | 404.591 | 498.988 | 517.228 |
| ResourceWithAmountContext            | Large         | 25     | 51.332  | 151.331 | 149.891 |
| ResourceWithOUtContext               | Large         | 25     | 2.050   | 2.271   | 2.518   |
| ResourceWithWL&AmountContext         | Large         | 25     | 48.452  | 135.032 | 137.056 |
| ResourceWithAmountFamiliarityContext | Large         | 25     | 159.232 | 372.712 | 371.158 |



## Results: 


We trained LSTM network of two sizes with configuration labelled as medium and large in Table above.  Both Networks have two layers and unrolled it has 35 steps. At its core the model consists of an LSTM cell that processes one task(executed by a particular resource) at a time and the network computes probability values for the next task in the sequence. The network memory state is initialized using a vector of zeros.  The network updates itself after reading each task in the given the sequence. Because of computational resource constraints we processed the data in mini-batches of size 20. The medium LSTM network has 200 units per layer and its parameters are initialized uniformly in [−0.1, 0.1]. The medium model was trained for 13 epochs with a learning rate of 1.0 and after 4 epochs we reduce the learning rate by a factor of 0.8 after each epoch. The large LSTM network has 650 units per layer and we initialize its parameters uniformly in [-0.05,0.05].  We train the LSTM for 25 epochs with a learning rate of 1.0, and after 6 epochs we decrease it by a factor of 1/1.15 after each epoch. We clip the norm gradients of both network (normalized by minibatch size) at 5. Training this network takes about one and a half hour on an 2.4GHz Macbook Pro.   

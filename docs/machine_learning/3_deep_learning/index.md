# Deep Learning: From Theory to Practice

## Neural Network Foundations

### Basic Architecture
* **Neurons (Units)**
    - Sum of weighted inputs: z = Σ(wixi) + b
    - Activation function: a = f(z)

* **Common Activation Functions**
    1. **ReLU**: f(x) = max(0, x)
        - Fast to compute
        - Helps with vanishing gradient
        - Can cause "dying ReLU" problem

    2. **Sigmoid**: f(x) = 1/(1 + e^(-x))
        - Outputs between [0,1]
        - Can cause vanishing gradient
        - Used in binary classification output

    3. **Tanh**: f(x) = (e^x - e^(-x))/(e^x + e^(-x))
        - Outputs between [-1,1]
        - Zero-centered
        - Still has vanishing gradient issues

    4. **LeakyReLU**: f(x) = max(αx, x)
        - Prevents dying ReLU
        - α typically small (e.g., 0.01)

### Forward Propagation
```python
class Layer:
    def __init__(self, n_inputs, n_neurons):
        self.weights = np.random.randn(n_inputs, n_neurons) * 0.01
        self.biases = np.zeros((1, n_neurons))
    
    def forward(self, inputs):
        self.output = np.dot(inputs, self.weights) + self.biases
        return self.output
```

### Backpropagation
* **Chain Rule Application**
    - ∂L/∂w = ∂L/∂a * ∂a/∂z * ∂z/∂w
    - ∂L/∂b = ∂L/∂a * ∂a/∂z * ∂z/∂b

* **Gradient Descent**
    - w = w - α * ∂L/∂w
    - b = b - α * ∂L/∂b

## Loss Functions and Optimization

### Common Loss Functions

#### Classification
```python
def binary_cross_entropy(y_true, y_pred):
    return -np.mean(y_true * np.log(y_pred) + 
                   (1 - y_true) * np.log(1 - y_pred))

def categorical_cross_entropy(y_true, y_pred):
    return -np.sum(y_true * np.log(y_pred), axis=1).mean()
```

#### Regression
```python
def mse_loss(y_true, y_pred):
    return np.mean((y_true - y_pred) ** 2)

def huber_loss(y_true, y_pred, delta=1.0):
    error = y_true - y_pred
    is_small_error = np.abs(error) <= delta
    squared_loss = 0.5 * error ** 2
    linear_loss = delta * np.abs(error) - 0.5 * delta ** 2
    return np.mean(np.where(is_small_error, squared_loss, linear_loss))
```

### Optimizers

#### SGD with Momentum
```python
class SGDMomentum:
    def __init__(self, learning_rate=0.01, momentum=0.9):
        self.learning_rate = learning_rate
        self.momentum = momentum
        self.velocity = None
    
    def update(self, params, grads):
        if self.velocity is None:
            self.velocity = {k: np.zeros_like(v) 
                           for k, v in params.items()}
        
        for key in params:
            self.velocity[key] = (self.momentum * self.velocity[key] - 
                                self.learning_rate * grads[key])
            params[key] += self.velocity[key]
```

#### Adam
```python
class Adam:
    def __init__(self, learning_rate=0.001, beta1=0.9, beta2=0.999, epsilon=1e-8):
        self.learning_rate = learning_rate
        self.beta1 = beta1
        self.beta2 = beta2
        self.epsilon = epsilon
        self.m = None
        self.v = None
        self.t = 0
    
    def update(self, params, grads):
        if self.m is None:
            self.m = {k: np.zeros_like(v) for k, v in params.items()}
            self.v = {k: np.zeros_like(v) for k, v in params.items()}
        
        self.t += 1
        
        for key in params:
            # Momentum
            self.m[key] = (self.beta1 * self.m[key] + 
                          (1 - self.beta1) * grads[key])
            # RMSprop
            self.v[key] = (self.beta2 * self.v[key] + 
                          (1 - self.beta2) * grads[key]**2)
            
            # Bias correction
            m_hat = self.m[key] / (1 - self.beta1**self.t)
            v_hat = self.v[key] / (1 - self.beta2**self.t)
            
            # Update parameters
            params[key] -= (self.learning_rate * m_hat / 
                          (np.sqrt(v_hat) + self.epsilon))
```

## Deep Learning Architectures

### Convolutional Neural Networks (CNN)

#### Basic Components
1. **Convolutional Layer**
```python
class Conv2D:
    def __init__(self, filters, kernel_size, stride=1, padding='valid'):
        self.filters = filters
        self.kernel_size = kernel_size
        self.stride = stride
        self.padding = padding
        
    def forward(self, input):
        # Implement convolution operation
        pass
```

2. **Pooling Layer**
```python
class MaxPool2D:
    def __init__(self, pool_size=2, stride=None):
        self.pool_size = pool_size
        self.stride = stride or pool_size
        
    def forward(self, input):
        # Implement max pooling
        pass
```

#### Common Architectures
1. **VGG**
    - Stack of 3x3 convolutions
    - Max pooling reduces spatial dimensions
    - Dense layers for classification

2. **ResNet**
    - Skip connections
    - Batch normalization
    - Deep architecture (50+ layers)

### Recurrent Neural Networks (RNN)

#### Basic RNN Cell
```python
class RNNCell:
    def __init__(self, input_size, hidden_size):
        self.Whh = np.random.randn(hidden_size, hidden_size) * 0.01
        self.Wxh = np.random.randn(input_size, hidden_size) * 0.01
        self.bh = np.zeros((1, hidden_size))
        
    def forward(self, x, h_prev):
        return np.tanh(np.dot(x, self.Wxh) + 
                      np.dot(h_prev, self.Whh) + self.bh)
```

#### LSTM Cell
```python
class LSTMCell:
    def __init__(self, input_size, hidden_size):
        # Initialize gates
        self.forget_gate = Layer(input_size + hidden_size, hidden_size)
        self.input_gate = Layer(input_size + hidden_size, hidden_size)
        self.output_gate = Layer(input_size + hidden_size, hidden_size)
        self.cell_gate = Layer(input_size + hidden_size, hidden_size)
        
    def forward(self, x, h_prev, c_prev):
        # Concatenate input and previous hidden state
        combined = np.concatenate((x, h_prev), axis=1)
        
        # Gate computations
        f = sigmoid(self.forget_gate.forward(combined))
        i = sigmoid(self.input_gate.forward(combined))
        o = sigmoid(self.output_gate.forward(combined))
        c_tilde = np.tanh(self.cell_gate.forward(combined))
        
        # Update cell and hidden state
        c = f * c_prev + i * c_tilde
        h = o * np.tanh(c)
        
        return h, c
```

### Transformers

#### Self-Attention Mechanism
```python
def scaled_dot_product_attention(Q, K, V, mask=None):
    d_k = K.shape[-1]
    attention_scores = np.dot(Q, K.T) / np.sqrt(d_k)
    
    if mask is not None:
        attention_scores += (mask * -1e9)
    
    attention_weights = softmax(attention_scores)
    output = np.dot(attention_weights, V)
    
    return output, attention_weights
```

#### Multi-Head Attention
```python
class MultiHeadAttention:
    def __init__(self, d_model, num_heads):
        self.num_heads = num_heads
        self.d_model = d_model
        assert d_model % num_heads == 0
        
        self.depth = d_model // num_heads
        
        self.wq = Layer(d_model, d_model)
        self.wk = Layer(d_model, d_model)
        self.wv = Layer(d_model, d_model)
        
        self.dense = Layer(d_model, d_model)
```

## Advanced Topics

### Regularization Techniques

#### Dropout
```python
def dropout(x, keep_prob):
    mask = np.random.binomial(1, keep_prob, size=x.shape)
    return (x * mask) / keep_prob
```

#### Batch Normalization
```python
class BatchNorm:
    def __init__(self, num_features, eps=1e-5, momentum=0.1):
        self.eps = eps
        self.momentum = momentum
        self.running_mean = np.zeros(num_features)
        self.running_var = np.ones(num_features)
        
    def forward(self, x, training=True):
        if training:
            mean = np.mean(x, axis=0)
            var = np.var(x, axis=0)
            
            # Update running statistics
            self.running_mean = ((1 - self.momentum) * self.running_mean + 
                               self.momentum * mean)
            self.running_var = ((1 - self.momentum) * self.running_var + 
                              self.momentum * var)
        else:
            mean = self.running_mean
            var = self.running_var
        
        # Normalize
        x_norm = (x - mean) / np.sqrt(var + self.eps)
        return x_norm
```

### Transfer Learning
1. **Feature Extraction**
    - Freeze pre-trained layers
    - Add new classification head
    - Train only new layers

2. **Fine-Tuning**
    - Start with pre-trained model
    - Unfreeze some/all layers
    - Train with small learning rate

### Model Deployment

#### Model Optimization
1. **Quantization**
    - Reduce precision of weights
    - Integer-only inference
    - Reduced memory footprint

2. **Pruning**
    - Remove unnecessary connections
    - Maintain accuracy
    - Reduce model size

3. **Knowledge Distillation**
    - Teacher-student architecture
    - Transfer knowledge to smaller model
    - Maintain performance

## Best Practices

### Training Tips
1. **Learning Rate Selection**
    - Use learning rate finder
    - Implement learning rate scheduling
    - Monitor training dynamics

2. **Batch Size Selection**
    - Memory constraints
    - Training stability
    - Generalization impact

3. **Initialization**
    - Xavier/Glorot initialization
    - He initialization for ReLU
    - Careful with deep networks

### Debugging Strategies
1. **Loss Not Decreasing**
    - Check gradients
    - Verify data preprocessing
    - Inspect learning rate

2. **Overfitting**
    - Add regularization
    - Increase training data
    - Reduce model capacity

3. **Underfitting**
    - Increase model capacity
    - Reduce regularization
    - Check for data issues

### Performance Optimization
1. **Memory Management**
    - Gradient accumulation
    - Mixed precision training
    - Efficient data loading

2. **Training Speed**
    - Parallel processing
    - GPU utilization
    - Efficient data pipeline

Remember:
1. Start simple and gradually increase complexity
2. Monitor training metrics carefully
3. Use appropriate regularization techniques
4. Consider computational resources
5. Test thoroughly before deployment
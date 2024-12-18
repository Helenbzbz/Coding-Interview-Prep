```
def stochastic_gradient_descent(X, y, lr=0.01, epochs=100, batch_size=1): """ Perform Stochastic Gradient Descent on a dataset. Parameters: - X: np.ndarray, feature matrix (n_samples x n_features) - y: np.ndarray, target values (n_samples) - lr: float, learning rate - epochs: int, number of iterations - batch_size: int, number of samples per batch Returns: - weights: np.ndarray, learned weights - bias: float, learned bias """ 
	
	# Initialize weights and bias 
	n_samples, n_features = X.shape 
	weights = np.zeros(n_features) 
	bias = 0 
	
	# SGD loop 
	for epoch in range(epochs): 
	# Shuffle data for randomness 
		indices = np.arange(n_samples) 
		np.random.shuffle(indices) 
		X = X[indices] 
		y = y[indices] 
		for i in range(0, n_samples, batch_size): 
			# Select a mini-batch 
			X_batch = X[i:i+batch_size] 
			y_batch = y[i:i+batch_size] 
			# Compute predictions 
			y_pred = np.dot(X_batch, weights) + bias 
			# Compute gradients 
			gradient_w = -(2 / batch_size) * np.dot(X_batch.T, (y_batch - y_pred)) 
			gradient_b = -(2 / batch_size) * np.sum(y_batch - y_pred) 
			
			# Update weights and bias 
			weights -= lr * gradient_w 
			bias -= lr * gradient_b 
	return weights, bias 
	

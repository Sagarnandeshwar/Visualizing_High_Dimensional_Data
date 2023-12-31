# Visualizing High Dimensional Data
We implemented t-SNE and SNE from scratch using numpy and evaluated the performance of them with 12 other visualization techniques, Isomap, LLE and UMAP. 

## Principal Component Analysis (PCA) 

The goal of principle component analysis (PCA) is to reduce the dimensionality of a data set with many connected variables while maintaining as much variance as possible. By projecting the original data in the direction where the variance between data points is largest, PCA turns a set of possibly correlated variables into a set of uncorrelated linear combinations of those variables called principal components. The outcome is a two-dimensional scatter plot, which makes data easier to understand. When utilising PCA, there are a few issues to consider. For starters, the technique is sensitive to the data set’s varying scale. As a result, mean centering and using the correlation matrix, which is normalised, is recommended. If the data is not normalised, the characteristics on the biggest scale will dominate the new main components. In addition, PCA isn’t ideal for classification. Finally, a cumulative explained variance threshold must be specified or modified. 

## Isometric Mapping (Isomap) 
Isomap is a non-linear dimensionality reduction method based on spectral theory that aims to keep geodesic distances in the lower dimension intact. Isomap begins by putting together a neighbourhood network. The approximate geodesic distance between all pairs of points is then calculated using graph distance. The low dimensional embedding of the dataset is subsequently discovered using eigenvalue decomposition of the geodesic distance matrix. The Euclidean metric for distance remains true in non-linear manifolds if and only if the neighbourhood structure can be approximated as linear. Euclidean distances can be deceiving if the vicinity has holes. In contrast, if we follow the manifold to estimate the distance between two points, we will get a better indication of how far or near two points are. 

## Stochastic Neighbor Embedding (SNE) 
The high-dimensional Euclidean distances between datapoints are converted into conditional probabilities that express similarities in SNE. The conditional probability, p j|i, that xi would choose xj as its neighbour if neighbours were chosen in proportion to their probability density under a Gaussian centred at xi is the similarity of datapoint xj to datapoint xi. For close datapoints, p j|i is rather high, whereas for far apart datapoints, p j|i is nearly infinitesimal (for tolerable values of the Gaussian variance, I The conditional probability pj|i is calculated mathematically as follows: 

![9](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/9.png)

Our 2D representation of X is matrix Y, which is a Nx2 matrix. We may construct distribution q based on Y in the same way that we constructed p. This is defined as: 

![8](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/8.png)

Our ultimate goal is to select points in Y that produce a conditional probability distribution q that is comparable toThis is accomplished by lowering a cost: the difference in KL between the two distributions. We wish to keep this cost to a minimum. We’re just interested in the gradient with regard to our 2D representation Y because we’re going to apply gradient descent. Perplexity is a parameter that we set in SNE (and t-SNE) (usually between 5 and 50). The i’s are then set so that the perplexity of each row of P is equal to our desired perplexity – the parameter we selected. The greater the i the closer the probability distribution is to having all probabilities equal to 1/N. So, if we desire more perplexity, we’ll increase the size of our ’s, which will lead the conditional probability distributions to flatten. This effectively raises each point’s number of neighbours. To verify that the perplexity of each row of P, Perp(Pi) = target perplexity, we just execute a binary search over each i until Perp(Pi) = our target perplexity. Perplexity Perp(Pi) is a monotonically growing function of i hence this is possible. We use a matrix of negative euclidean distances and a target perplexity to find all i’s. We execute a binary search over all possible values of i’s for each row of the distances matrix until we locate the one that results in the target perplexity. We then return a numpy vector with the found optimal i’s. By reducing the gradient of the cost C with respect to Y until convergence, we might obtain a good 2D representation Y. However, because SNE’s gradient is more difficult to implement, we’ll instead employ Symmetric SNE. We minimise a KL divergence over the joint probability distributions with entries pij and qij, rather than conditional probabilities pi|j and qi|j, in Symmetric SNE. So now we have all of the information we need to calculate Symmetric SNE. 

## t-Distributed Stochastic Neighbor Embedding (t-SNE) 

SNE produces reasonably good visualizations, but it is plagued by a difficult-to-optimize cost function and an issue we call the "crowding problem." The t-SNE project tries to solve these issues. The cost function used by t-SNE differs from that used by SNE in two ways: it uses a symmetrized version of the SNE cost function with simpler gradients, and it computes the similarity between two points in the low-dimensional space using a Student-t distribution rather than a Gaussian distribution. To solve both the crowding and optimization concerns of SNE, t-SNE uses a heavy-tailed distribution in low-dimensional space.  

It’s easy to transition from Symmetric SNE to t-SNE. Only the way we define the joint probability distribution matrix Q, which includes entries qij, differs, and gives us the following: 

![7](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/7.png)

This is obtained by assuming that the qij follow a one-degree-of-freedom Student t-distribution. Essentially, this indicates that the technique is nearly invariant to the low-dimensional mapping’s general scale. As a result, the optimisation works the same for very far apart locations as it does for ones that are closer together. 

 ## Locally Linear Embedding (LLE) 

The LLE algorithm begins by identifying a set of each point’s closest neighbours. It aids in representing the point as a linear combination of its neighbours after locating the nearest neighbours by determining the weights set for each point. It also employs an eigenvector optimization technique in the final step of determining the low dimensional embedding for points. Finally, each point may be described as a linear combination of its neighbours thanks to the final step. There is no internal model in LLE. 

The LLE employs the barycentric coordinates of Xi to compute the neighbour Xj of any point Xi. The algorithm reconstructs the original point using the linear combination. The linear combination to point Xi is given by the weight matrix Wij. The algorithm’s main purpose is to create data of dimensions D of size d, where D ≫ d. The idea behind the creation of neighbourhood preserving maps is that the same weight matrix used to rebuild original points in data with dimensions D may also be used to reconstruct the same original point in data with dimensions d. In addition, the cost function minimization is considered, and each point Xi in the D dimensional data is mapped onto a point Yi in the D dimensional data. 

## Uniform Manifold Approximation and Projection (UMAP) 

Uniform Manifold Approximation and Projection (UMAP) is a dimension reduction approach comparable to t-SNE that can be used for visualisation as well as non-linear dimension reduction. The data is uniformly distributed on a Riemannian manifold, the Riemannian metric is locally constant (or can be estimated as such), and the manifold is locally connected, according to the algorithm. It is possible to model the manifold with a fuzzy topological structure based on these assumptions. The embedding is discovered by looking for the closest possible equivalent fuzzy topological structure in a low-dimensional projection of the data. 

## Dataset 

### MNIST 
The Modified National Institute of Standards and Technology dataset is referred to as the MNIST dataset. It’s a collection of 60,000 small squared grayscale photos of handwritten single numbers ranging from 0 to 9. Total dimensions are 784. 

### Fashion MNIST 
The Fashion MNIST Dataset is an MNIST-like dataset of 70,000 28x28 labeled fashion images. It is divided into two subsets: one for training(60,000) and the other one for testing(10,000). Each row in the dataset is a separate image. The first column is the class label whereas the remaining columns are pixel numbers (784 total). Each value is the darkness 105 of the pixel (1 to 255)[3]. Total dimensions are 784. 

### Olivetti Faces 
There are ten different images of 40 distinct people in this dataset. There are 400 face images in total. Face images were taken at different times, varying lighting, facial expressions and facial details. All images have black backgrounds 

![6](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/6.png)

## Result 

![1](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/1.png)
![2](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/2.png)
![3](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/3.png)
![4](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/4.png)
![5](https://github.com/Sagarnandeshwar/Visualizing_High_Dimensional_Data/blob/main/images/5.png)

The comparison graphs are mentioned in the coding section in detail. We see that UMAP performed better cluster separation compared to other. Although this reduction technique wasn’t mentioned in the research paper, but we wanted to add this to show why UMAP is now widely used as a visualization technique. After UMAP, tSNE had a superior performance over other techniques like SNE, LLE and Isomap. Since tSNE is the primary focus of this report, we impelemented tSNE and SNE from scratch. We will be comparing the mathematical advantage of tSNE over SNE and why the former performed better than the latter. In addition, we’ll also discuss the supriority of the modern reduction technique i.e. UMAP over tSNE and why it was able to significantly separate the clusters. 

 When comparing tSNE and SNE reduction technique, tSNE has a higher mathematically advantage over SNE because, former uses a t-distribution and a slight addition to KL Cost function which reduces a "crowded-problem" in the SNE convergence. This special "t-distribution" has an added advantage that points closer to the datapoint in focus i.e. xi, similar closer points becomes "attracted" to the target datapoint while other dissimilar classes datapoints, are repelled more strongly. In addition, since the distribution has a longer tail-end, further points become more further apart. 

UMAP is relatively new technique introduced in the industry and as we can see, it was able to outperform all the other techniques, in terms of visualizing classified datapoints, including tSNE. The reason for this superiority is because UMAP has a mathematically superior cost function as it uses "Binary Cross-Entropy", compared to tSNE which uses "KL Divergence" cost function. KL divergence performs well, if datapoints are closer together in high-dimension. Because if they are closer together in high-dimension, then to minimize the cost-function in lower-dimension, they have to be closer-together. However, if in the higher-dimension, datapoints are further apart, then in low-dimension, whether datapoints are further or closer together, then minimization of cost function becomes irrelevant, hence it only preserves local structure. Whereas, UMAP is able to preserve global structure due to its cross-entropy cost function as points further in high-dimension, and vice versa in order for gradient descent to converge. 

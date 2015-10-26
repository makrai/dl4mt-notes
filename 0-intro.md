#by Kevin Duh,
  * NAIST, Japan
    * (will be affiliated with JHU, US during the winter school)

##DL tutorial
* DL is
  * a family of methods that uses
  * ...
  * to learn hight-level representations
* opposed to engineered features
* Lee et al 2009 vision, automatically learning features
* softmax
  * multinomial variant of sigmoid
* toolkits
  * Theano, Torch ,pylearn2, Caffe, CNTK, CNN
* Bengio sitting (Bengio 2009)
* too many layers -> overfitting
* layer-wise pre-training
  * Hinton (2006) -> interest return in deep learning
  * for image recog, fist comp graphics! (P(X) instead of P(Y|X))
  * why does it work? Erhan (2010)
  * is necessary?
    * 2006 yes
    * now no
    * ...in speech, still used
  * word2vec is a kind of pre-training? Che and Manning (2014)

## building blocks of deep architectures
### RBMs and unsupervised learning
* RBM
  * p(x,h) = 1/Z_Theta exp(-E_Theta(x,h)
       * = 1/Z_Theta exp(x^TWh + b^Tx + d^Th
     * x, h \in {0,1}
     * 1/Z => probab distri
     * W correlation?/interaction betw x,h
     * assume b = d = 0
     * components of x are independent given h
  * max likelihood training
    * the derivative of the log-likelihood
    * increases prob of x(m)
    * decreases...
  * contrastive divergence algo
    * algo
      * 1. x^(m), W given
      * 2. sample h from~
      * 3. sample x~
      * 4. sample h~
      * 5. wij += gamma(x~_j^(m) h~_j - x_j h_j^(x))
    * pictorial
      * x \mapsto P(x)
  * distributed repr
  * layer-wise pre-training
    * intuition: Hinton, x and Teh
  * deep belief net = stacked RBM (Hinton et al 2006)
    * p(x) = sum_{h, h', h''} p(x|h) p(h | h') p(h'|h'')
    * generatiion
    * init MLP with RBMs
      * fine-tune with backprop

### Auto-encoder (AE)
  * cheap replacement for RBMs
  * stacked AEs
  * h is continuous, determinic
  * denoising
    * noise construction is domain specific
  * recurrence
    * in the hidden layer
  * lstm
    * gate
      * forget (i.e. remember), input, output
    * Gers et al (2002)
  * convolution
    * pooling: elbambultam

## tricks of the trade
* optimization
  * making sgd work
    * learning curve
      * validation error increases from the start:
        * regularization needed
        * figure on learn rate \gamma from Schaul et al 2013
    * batch
      * mx mx multi insted of vector mx multi, faster
    * random init
      * in the range where the abs deriv of sigmoid is not too small
      * fanin: in-degree
      * paper: efficient back prop
    * non-convex
      * momentum
      * adaprive learning rate:
        * adagrad: each weight has a learning rate
        * adadelta: no gloval learning rate that would have to be tuned

###second order (Newton) methods
* quasi-Newton: L-BFGS
  * conjugate gradient
* drop-out
  * error goes on decreasing
* multi-task learning
  * Liu et al (2015) 

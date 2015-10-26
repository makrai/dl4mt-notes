#Neural LMs and TMs for SMT I 
* Hermann Ney 

## organiz of labs
* lab 1: lang mod
* lab 2: mt

## hlt
* asr, ocr (in seq), and mt: sequ proc
* projects, RWTH team 
* motiv from 10 years ago
  * skype translation?

## stat in hlt
* notation
  * pr: reality
  * p: prob approxed by model
* input X, output W
* bayesian decision rule
  * loss func L
  * Schülter Nussbaum 12, Bahl & Jelinek 83
* generative vs discriminative
  * gen p(X,W)
  * discriminative p(W|X)
* asr
  * Bahl & Jelinek 83
* hmm
  * speech, handwriting, trans
    * reference model
  * until 2011, emission prob was GMM
  * simil between spech and trans in hmm: phoneme lenght vs word reordering
  * p40 history of acou
  * p42 output nodes: 50 phonemes, now more phon feat
  * question (misinterpreted?)
    * google brain
      * lstm in acou
* handwiriting 
  * database
* trans
  * target x source
* stat to asr
  * some parts of stat are irrelevant
    * unbiassed estimate...
* ingredients
  * perform meas
  * prob model
    * depend within and between X and W
    * elementary observs (GMM...) and string
  * training crit
  * bayes decision rule

## NN and stat
* the point will be:
  * is NN related to bayes?
* interpret at the first renessaince of nn: 1986b
  * human biol brain
  * parallel 
  * model
* classical archit
  * classif
* activ func
  * sigmoid and tanh init is different in practice
  * rectified: some peole saw positive results
* decision rule
  * c^ ?= argmax_c p(c,x)?
    * in isolation, yes
    * context see later
* train crit
  * sq error p(c,x) \in R (may be negative, large,...)
  * binary cross-entropy \in {0,1}
  * cross-entropy: p is probab distri
  * in all the three cases, error is minimal if p(c,x) = pr(c|x)
  * interpret
    * gradient search, backprop
      * finds a local optimum
        * in practice, local optima are at the same level

### Softmax (3.4)
* connection to Gaussian (generative) model
* softmax = class posterior distri of a Gauss model
* cross-entro is called mmi in asr
* mlp and feature functions
* last 5-6 years in neural speech

### rnn for sequ proc
* h_t = activ(h_{t-1}W_1 + x_t W_2)

##lang model (lm) p47
* in vitro eval of lms
  * perplexity
* Markov chain, cout models, smoothing
  * toolkit SRI

### nn for lm
* history from 1989
  * Schwenk 07: competitive
  * Sundermeyer (2012): tool
* feedforward mlp
  * projection layer
    * no sigmoid
  * (second) hidden layer with sigmoid \sigma
  * output: softmax S
* output layer
  * too large
  * word classes to make softmax more efficient
    * exchange algorithm: make_class? in Moses or Giza
    * TODO miért nem |C| = sqrt(|V|)
* recurrent
  * h_t = sig(A_2 x_t + R h_{t-1})
* lstm
  * Hochreiter & Schmidthuber 97, Gers & Schraudolph 02
  * <- vanishing gradient
    * (exploding gradient is still unsolved)
  * each node replaced by...
    * input, output, forget
    * cell state c, net output s
    * i, o, f in [0,1]
  * computation time
  * first successes in handwritten...
    * what do the gates do: see Karpathy
* interpol
* lm for smt
  * now: traditional translation with neural lm
  * perlexity correlates
    * bleu: good
    * TER: worse
* summary
  * in asr, this has been developed for 25 years
  * question: in recurrent, why to have 300-dim word repr instead of 1-of-V

### practicalities

## SMT
### conventional
* hist
  * stat was forbidden during Chomsky, even in the 1980s
  * IBM stopped in 94
  * ...
* trans
  * what is called "decoding" in analogy to hmm
    * lex choice
    * reoredring
    * synt and sem in target (and src)
  * f: foreign, French
  * fertility (termékenység)
  * IBM-3-5 no closed form of train, only heu
  * IBM, then giza++
  * phrase
    * there may be e words without aligned f word
  * log-lin comb of models: max ent
    * theoretically a softmax over sentences
    
### nn for mt
* history: not new (1997)
  * inverted alignment is more convenient in generation (not in training)
  * the component ... from page 89
* h(E,F): alignment is implicit
  * n best list: 500-1K candids
  * dev: lambda trained
     * ^ weight of each module

#### rnn for smt
* bidirectional: future of src is included (Schuster & P?)
* number of params
* ongoing and future work p114
  * better lexicon model
  * integrated re-alignment + lexicon

### practical


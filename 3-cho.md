* Kyunghyun Cho
* NYU
* used to be post-doc at Bengio

## Intro nlu
* without deep learning
* with deep learning
  * Skinner: Verbal behaior

## lm
* Jelinek 1988 Every time I fire a linguist...
* "likelihood" contains truth
* n-gram
  * I like a lama.
* neural 1999/ Bengio 00 
* using the same embedding mx in a neural n-gram model 
  * can be thougt of as a kind of convolution
* cbow a bit worse in perplex than ff nlm Bengion 00

### non-markovian
* other languages: SOV, e.g. Korean
  * V far from S
* p( |w1, w2) = g(h) h: hidden state
* bias init: log unigram prob
* recurrent became popular with Mikolov 11
* training rnnlm
  * log-likelihood is a func of params, not examples
  * minibatch sgd
    * never until convergence, that would be overfitting
  * backprop through time

### gated recurrent units
* expolode/vanish grad Bengio 94
  * clipping the gradient 
    * not written in the papers until 2011--2013
    * works only for exploding
    * Markov returns? (Eduard)
    * clip each step: Alex Craves?
    * vanish, Pascanu 13
      * gain is not clear
      * it can hurt 
        * force the model to hallucinate long-term depend
      * comput is expens
* temporal short-cut connections 
  * we donot know how long the sequ is
  * h_t~: candidate for h_t
  * resest \approx forget
  * compar to lstm
    * Cho et al (14) did not like lstm
    * both adress vanishing, none adresses explode
      * explode <- clipping
    * ...
  * long-term depend (ltd) captured on toy data for ltd by both
  * an empir explor of rnn archits

## mt
### fun trivia
* Warren Weaver 1949 smt
* Bronw et al 1991 smt reinvented

### argmax p(e|f) = argmax p(f|e) p(e)
* mt book by Philip Colin?
* history
  * 1997 Spain Valencia and 
    * Forcada & Neco 1997
      * encoder-decoder, recursive hetero-assoc 
    * Castano & Casacuberta 1997
  * Kalchbrenner & Blunsom 2013 cowboy
  * Sutskever et al 2014
  * Cho et al 2014
* embedding: slicing rather than product 
  * google brain: lstm
    * competitive with phrase-based
  * cho et al: gated lu

### decoding step
  * beam search is a good idea
    * tok-k candidates each step
* compar google brain and cho et al
  * google model is 100x larger
* reverse the rnn <-
  * the task of the trans model is to provide a good beginning of the sentence
    * that the lang model can finish
    * at least en-fr
  * latest word are better represented
* sentence length
  * determine from source lenght? 
    * no, it would be gibberish of specified lenght
  * we have good data for </s>: in every sentence

### attention
* human
  * whole sentence
  * come back to 2--3 phrases
* annotation vectors: to directions
* attention weight a: prob distribution over the words in the sentence
* Gábor attention with unidirectional rnn?
  * do not work well because of multiply accuring words
  * Stanford Luon
* why not c = max a_t? 
  * because max is not differentiable
  * Wirtschaftswachstum is already encoded in the vector for Economic
* results
  * p22 BLEU scores
  * 30, 50: max sentence length 
  * 50 is inited with 30?
* vocab limit
* automatic evaluation metric
  * there is some correlation betw BLEU and human
  * image desc...: worse
  * first two are roboust, the rest reversed

### large target vocab
* complex is domin by output p23
* avoid cpu-gpu communication 
* submx
  * training
    * subsets with managable vocab
  * translation 
* why not bother src vocab
  * src embedding: only positive words are updated
    * grad W = delta [0 0 ... 1 0 ... 0 ]

### subword
* word is not so great in all langs
  * e.g. Turkish
* Sennrich 2015
* morphessor
  * mess
  * nobody left working on it in 10 years
* <-> tgt
* solu: char level
* vision: earlier: ml started a bit higher than the pixel level
* highway: 10--20 hidden layers <- essential for char-based to work
  * for English-like orthography
  * gates (approx lstm) in ff layer
* 100K most freq word + ...
* char convol -> fixed dim -> highway
  * n-gram convol feat for some of each n
* more efficient
* Chinese: which unit to get char n-gram from?
* to English is worse than from English
  * neural: fluency is smoother than with phrase-based

### tgt lm
* how to use external lang mod
  * debates in Montreal
  * non-stationary weighting between lm and trans model
* esp when src is low-srcd like Turkish
* incompat with n-gram count lm
* g_t is 1x1 but vector is better

### general discuss
* Rasnik not fond of nmt
* Manning & Schütze foundat smt
* let the data say that some param is unnecess
* net topology engineering
  * uniform prior wont work
  * just like human brain
* lecture of past 10 years
  * be more afraid of constrained opt than of nonconv opt
  * local minima tend to be equivally good
* non-neu
  * featuress have to be lineraly compa{ra/ti?}ble

### conclusion
* Hirschberg & Manning (2015)
  * overview of nlp in Nature
* future
  * multiling
    * earlyer: pivot lang
  * discourse-level
  * other than mt
* p42 skipped
* connectionist approach to nlu
  * Hutchins and Somers (1992) ch on future
* p50 columns lm, biling, and trans
      * ^ co-occur    ^ replacement
  * no univ embed    
* all the code is open-source
  * except for google
* non-language
  * attention in image
  * video
  * speech is messier

## lab by Orhan Firat
* orthogonal init
* tied varibales in different layers
* theanorc
* other open-src implement of attention
  * GroundHog
    * not in develop but good
  * in blocks
    * https://github.com/mila-udem/blocks-examples/tree/master/machine_translation
* work-flow
  * build comput graph
  * init vars
  * updates
  * monitors
* tparams: theano shared params

### mt with attention
* two encoders with softmax model replaced
* bidirect rnn
* init the init state of the hidden state z of the decoder
  * it matters a lot
  * we gonna learn how to init
* decoder
  * h>.shape is   n_time, n_sample, n_dim
  * h<.shape is   n_time, n_sample, n_dim
  * ctx.shape is  n_time, n_sample, 2 * n_dim
* conditional rnn with attention
  * conditioned on the context (in addition to the last word)
* opt_ret
  * for visu, debug,..
* readout-layer z
* aswer to the question
  * 3x slice: fwd, bkd, decode
  * bellow means above i.e. u
* do computation in/out of scan for efficiency
* lower-case: bias, upper-case: mx
* pctx projected (from input) context
* alpha = U_att * tanh(stat_below W_i_att + h_{t-1} W_d_att + ctx W_c_att) +
                                                                         * c_att
* regularize alpha weights: only in image caption
* exp (like line 431) has to be normalized
* theano sigmoid: only in 2d mx
* gen_sample: beam search
* fast iterator
  * sentences should be sorted by lenght within a range of some batches 
  * to minimize padding
  * it is done in blocks?  
#### session 3
* given lm, tm
  * n-best list..

* 4/75
* Michael Lysaght
* cfp mt special issue on dl4mt

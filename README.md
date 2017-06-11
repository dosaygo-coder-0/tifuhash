# tifuhash

TIny Fast Universal Hash - that's the aim anyway

# Overview

Tifuhash benefits:
  - is a novel construction based on continued fractions and Egyptian fractions
  - passes the two most demanding bias tests in existence: PractRand for RNGs and SMHasher for hash functions

# Testing Results

Tifuhash passes two key tests for bias: [PracRand](http://pracrand.sourceforge.net/) for RNGs and [SMHasher](https://github.com/aappleby/smhasher) for non-cryptogrpahic hash functions

The test results for SMHasher are in [tifuhash.smhasher.results.txt](https://github.com/dosaygo-coder-0/tifuhash/blob/master/tifuhash.smhasher.results.txt) and the results for PractRand are in [tifuhash.practrand.results.txt](https://github.com/dosaygo-coder-0/tifuhash/blob/master/tifuhash.practrand.results.txt)

# Installing
 
 `npm install tifuhash`
 
 # Using
 
 ```js
 const tifu = require('tifuhash');
 
 const message = 'The medium is the message.';
 const number = 333333333;
 const float = Math.PI;
 
 tifu.hash( message ); // OK
 tifu.hash( number ); // OK
 tifu.hash( float ); // OK
 tifu.hash( ); // empty message - OK
 ```
 
# Construction

- Novel: Based on continued fractions and Egyptian fractions
- Small: Uses two 64 bit floating point numbers for calculation
- Versatile: Can hash arbitrary length string messages, integers and floats
- Fast: Apart from using 64 bit FPA, it has only a few operations per message byte ( it can probably be efficiently implemented in hardware)
- Amazing Properties: Somehow, this tiny, maths-operation-only hash passess PractRand ( WTF! ), when iterated on its own output and when the *entire* resulting hash stream is concatenated together to make a binary output.
- Novel: Searching Google for "continued fraction hash algorithm" and "Egyptian fraction hash algorithm" produces no other efforts
- Novel: using division, or using floating point division, and discarding the high-order-bits ( as we do here ) is not used nor studied so much, if at all, in hash construction
- Universality: It can be parameterized by:
  - Setting the initial state of the two 64 bit floats
  - Including an additional constant summand at each step ( perhaps a different one for each 64 bit state )
  - Including different additional summands at each step ( maybe the ouput of a LFSR )
 - Because it has good properties ( independence of bits as demonstrated by passing PractRand ), and can be parameterized, we hypothesize that it is universal, possibly even strongly universal.
 - Tiny: Yep, it's memorizable and only a few lines of code
 
 ## Tifuhash limitations and opportunities for improvement
 
  - current implementations, while still fast, are very slow compared to existing top hash functions, even tho the code of tifuhash is simple
    - this can probably be improved in optimized implementations, but floating point sets a hard limit on how fast tifuhash can be
  - uses floating point extensively, so hashes can differ depending on implementation and architecture
    - this may be able to be improved using techniques developed to allow other floating point dependent calculations to be reproducible across languages and architectures
 
 # Parameterization and Universality

 Universal hash algorithms define a family of hash algorithms. Tifuhash is designed to be universal, and while not API for parameterizing the algorithm is available yet, that will come. I think we can probably use the "seed" construction, used by SMHasher, to setup the intial state, as one way to explore parameterization. Accepting seeds into the initial state is already implemented in the C++ reference code.

 Also no built in method for generating real entropy in order to form parameters to universal hash parameterization, but that will probably come, or use code I developed for `dosycrypt`
 
 # Links

 - [tifuhash on npm](https://www.npmjs.com/package/tifuhash)

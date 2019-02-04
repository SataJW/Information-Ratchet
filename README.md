# Information-Ratchet

## Requirements

+ numpy
+ scipy

## Usage Example

### 2-States Ratchet

Here, we will simulate a 2-states information ratchet processing information on a input tape sequence of '1' and '0' generated with a statistical bias (bias coin flip). We wish to find the change in information content of the tape after being processed by the information ratchet, as well as the average work being extracted.

First import the module.
```python
from finitestate import Tape, Ratchet
```

Next, define the input tape and ratchet with an arbitrary transition matrix.
```python
# Define input tape of length 5000 and generate sequence with statistical bias
inputTape = Tape(5000)
inputTape.generateSequenceBias(0.9)

# Define ratchet's transition matrix and ratchet
p = 0.3
q = 0.7
transMat = np.matrix([[0, 1 - p, 0, 0],
                      [1, 0, q, 0],
                      [0, p, 0, 1],
                      [0, 0, 1 - q, 0]])
ratchet = Ratchet(transMat)
```

Next, we can start the run.
```python
# Start run on inputTape with initial ratchet state 0
outputTape, inStates = ratchet.startFixedTransitionRun(inputTape, 0)
```

We can then find the change in entropy of the tape after being processed by the ratchet, as well as the total work done due to the transitions of the joint states.
```python
inputTape.calcEntropyRate()
outputTape.calcEntropyRate()

deltaEntropy = outputTape.entropyRate - inputTape.entropyRate

totalWork = ratchet.calcWork(inStates, outputTape.markovChain)
aveWork = totalWork / inputTape.length
```

### Analytical and Numerical Result Comparison

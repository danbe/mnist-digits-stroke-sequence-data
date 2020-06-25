This is a clone of the original MNIST-digits-stroke-sequence-data, converted to BSON
See the [wiki page](https://github.com/edwin-de-jong/mnist-digits-stroke-sequence-data/wiki/MNIST-digits-stroke-sequence-data)

After loading the original code into dataframes, it was saved to BSON:
```python
import pickle
import bson
train = pickle.load(open('train.df', 'rb'))
test = pickle.load(open('test.df', 'rb'))
testdict = test.to_dict()
traindict = train.to_dict()

trainlist = [(traindict['label'][k], traindict['dx'][k].tolist(), traindict['dy'][k].tolist()) for k in traindict['label'].keys()]
testlist = [(testdict['label'][k], testdict['dx'][k].tolist(), testdict['dy'][k].tolist()) for k in testdict['label'].keys()]
open('mnistStroke.bson', 'wb').write(bson.dumps({'train':trainlist, 'test':testlist}))
```

The data can be imported in Julia using the following code:
```julia
using BSON
using UnicodePlots
mnist = BSON.load("mnistStroke.bson")
i=1 # index into :train and :test
label, dx, dy = mnist[:train][i]
dx = convert(Array{Int,1}, dx)
dy = convert(Array{Int,1}, dy)
lineplot(cumsum(dx), -cumsum(dy), title=string(label))
```

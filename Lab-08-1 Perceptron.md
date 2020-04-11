

```python
import torch
```


```python
device = 'cuda' if torch.cuda.is_available() else 'cpu'

# for reproducibility
torch.manual_seed(777)
if device == 'cuda':
    torch.cuda.manual_seed_all(777)
```


```python
# Train Data

X = torch.FloatTensor([[0,0], [0,1],[1,0],[1,1]]).to(device)
Y = torch.FloatTensor([[0], [1], [1], [0]]).to(device)
```


```python
# SLP 선언
linear = torch.nn.Linear(2,1, bias = True)    # 2 inputs, 1 output
sigmoid = torch.nn.Sigmoid()

model = torch.nn.Sequential(linear, sigmoid).to(device)
```


```python
criterion = torch.nn.BCELoss().to(device)
optimizer = torch.optim.SGD(model.parameters(), lr=1)
```


```python
for step in range(10001):
  optimizer.zero_grad()
  hypothesis = model(X)

  cost = criterion(hypothesis, Y)
  cost.backward()
  optimizer.step()

  if step % 100 == 0:
    print (step, cost.item())
```

    0 0.7273974418640137
    100 0.6931476593017578
    200 0.6931471824645996
    300 0.6931471824645996
    400 0.6931471824645996
    500 0.6931471824645996
    600 0.6931471824645996
    700 0.6931471824645996
    800 0.6931471824645996
    900 0.6931471824645996
    1000 0.6931471824645996
    1100 0.6931471824645996
    1200 0.6931471824645996
    1300 0.6931471824645996
    1400 0.6931471824645996
    1500 0.6931471824645996
    1600 0.6931471824645996
    1700 0.6931471824645996
    1800 0.6931471824645996
    1900 0.6931471824645996
    2000 0.6931471824645996
    2100 0.6931471824645996
    2200 0.6931471824645996
    2300 0.6931471824645996
    2400 0.6931471824645996
    2500 0.6931471824645996
    2600 0.6931471824645996
    2700 0.6931471824645996
    2800 0.6931471824645996
    2900 0.6931471824645996
    3000 0.6931471824645996
    3100 0.6931471824645996
    3200 0.6931471824645996
    3300 0.6931471824645996
    3400 0.6931471824645996
    3500 0.6931471824645996
    3600 0.6931471824645996
    3700 0.6931471824645996
    3800 0.6931471824645996
    3900 0.6931471824645996
    4000 0.6931471824645996
    4100 0.6931471824645996
    4200 0.6931471824645996
    4300 0.6931471824645996
    4400 0.6931471824645996
    4500 0.6931471824645996
    4600 0.6931471824645996
    4700 0.6931471824645996
    4800 0.6931471824645996
    4900 0.6931471824645996
    5000 0.6931471824645996
    5100 0.6931471824645996
    5200 0.6931471824645996
    5300 0.6931471824645996
    5400 0.6931471824645996
    5500 0.6931471824645996
    5600 0.6931471824645996
    5700 0.6931471824645996
    5800 0.6931471824645996
    5900 0.6931471824645996
    6000 0.6931471824645996
    6100 0.6931471824645996
    6200 0.6931471824645996
    6300 0.6931471824645996
    6400 0.6931471824645996
    6500 0.6931471824645996
    6600 0.6931471824645996
    6700 0.6931471824645996
    6800 0.6931471824645996
    6900 0.6931471824645996
    7000 0.6931471824645996
    7100 0.6931471824645996
    7200 0.6931471824645996
    7300 0.6931471824645996
    7400 0.6931471824645996
    7500 0.6931471824645996
    7600 0.6931471824645996
    7700 0.6931471824645996
    7800 0.6931471824645996
    7900 0.6931471824645996
    8000 0.6931471824645996
    8100 0.6931471824645996
    8200 0.6931471824645996
    8300 0.6931471824645996
    8400 0.6931471824645996
    8500 0.6931471824645996
    8600 0.6931471824645996
    8700 0.6931471824645996
    8800 0.6931471824645996
    8900 0.6931471824645996
    9000 0.6931471824645996
    9100 0.6931471824645996
    9200 0.6931471824645996
    9300 0.6931471824645996
    9400 0.6931471824645996
    9500 0.6931471824645996
    9600 0.6931471824645996
    9700 0.6931471824645996
    9800 0.6931471824645996
    9900 0.6931471824645996
    10000 0.6931471824645996



```python
with torch.no_grad():
    hypothesis = model(X)
    predicted = (hypothesis > 0.5).float()
    accuracy = (predicted == Y).float().mean()
    print('\nHypothesis: ', hypothesis.detach().cpu().numpy(), '\nCorrect: ', predicted.detach().cpu().numpy(), '\nAccuracy: ', accuracy.item())
```

    
    Hypothesis:  [[0.46586224]
     [0.4627448 ]
     [0.32562828]
     [0.32288203]] 
    Correct:  [[0.]
     [0.]
     [0.]
     [0.]] 
    Accuracy:  0.5

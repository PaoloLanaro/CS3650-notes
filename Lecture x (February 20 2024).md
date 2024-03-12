##### zoom recording for lecture February 16 

## Associative Cache

## Direct-mapped Cache

Page Tables for RAM (paging)
(special case of direct-mapped cache)

Memory Hierarchy 

Fast
---------------------------------------------------------------------------------------------->

registers                         CPU Cache                           RAM (lw, sw, over a hundred asm instructions)

Large
<---------------------------------------------------------------------------------------------

size of cpu cache can be anywhere from 32 kb to 8mb

L1 cache
L2 cache - (level 2)

ps aux -> show me all the process

Fully Associative Cache

| Address | Data block ("value") |
| ---- | ---- |
|  |  |
| 0xa400 | 0xa404; 0xa408; ... ;    |
|  |  |
Latency is the time for first byte to arrive

Bandwidth is \# of bytes per second

Optimization:
    Data block is more than one word



```cpp
int maxProfit(vector<int>& prices) {
  int currentDiff;
  int currMin = INT_MAX;
  int minPos = 0;
  for (int i = 0; i < prices.size(); ++i) {
    if (prices[i] < currentMin) {
      currMin = prices[i];
      minPos = i;
      continue;
    }
	if (currentDiff < prices[i] - minPos) {
	  currentDiff = prices[i] - minPos;
	}
  }
  return currentDiff;
  }
```

Fully Assoc. Cache

V (Valid) = 0 or 1
M (Modified) = 0 or 1

Cache initially; V = 0 everywhere

lw (read):
	Examine if in cache.
		If so, done 
		Else goto RAM for "value" and allocate a free cache line,
		(Set V = 1, set address, data, M = 0)

sw (write):
	Examine if address in cache
	If so, update data, set M = 1, Done.
	Else; (address not in cache)
		Option 1:  Choose victim with M = 0, evict, then use address data, M = 1.
		Option 2,3,...,:


### Comparators use lots of space so a fully associate cache is usually small

Solution? Direct-Mapped Cache

The trick:
CPU has address. split the address into various fields.
# Midterm 1: answers

GitHub username: kolenkeee
Session code: maple-66
Fingerprint: facf2340

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude to explain the lectures, the rules and the formulas, and I used Google Colab to make my data and do the calculations. I wrote the answers myself and I can explain them.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.583 | 0.683 | 0.800 | 0.660 |
| B | 0.433 | 0.683 | 0.900 | 0.593 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.683 | 0.558 | 0.787 |
| A, k=5 | 0.800 | 0.682 | 0.882 |
| B, k=3 | 0.683 | 0.558 | 0.787 |
| B, k=5 | 0.900 | 0.799 | 0.953 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 37002 |
| A, k=5 | 47223 |
| B, k=3 | 25999 |
| B, k=5 | 28885 |

### 1d. Decision 

Chosen candidate: (B, 5)

I choose B with k=5. Its cost is $28885 a year. This is under the budget of $32900. Both A candidates are over the budget: (A, 3) costs $37002 and (A, 5) costs $47223.
The other candidate in the budget is (B, 3). Its recall is 0.683 [0.558, 0.787]. (B, 5) has 0.900 [0.799, 0.953]. The intervals do not overlap, so (B, 5) is really better.
Recall of (B, 5): EN 0.95, RU 0.95, KK 0.80. KK is the lowest, but with 20 questions the intervals overlap, so I am not sure.
Kazakh users are served worst, and they are 39% of the traffic.

### 1e. Next measurement (at most 60 words)
I would test (B, 5) on many more Kazakh questions, at least 100, because KK has only 20 questions and its interval is wide [0.584, 0.919]. If KK recall is really lower than EN and RU, I would change the chunks or the model for Kazakh.
## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.867 | KK, degraded | 0.767 | 0.591 | 0.882 | 17164 |
| L | 0.800 | RU, degraded | 0.567 | 0.392 | 0.726 | 5400 |

Personal-data pages per year sent abroad under H: 542025

### 2b. Memo (at most 300 words)
I recommend pipeline L, the one on our own server in Kazakhstan.

H is a little more accurate overall: 0.867 against 0.800 for L. But the intervals overlap ([0.809, 0.909] and [0.736, 0.852]), so I cannot say that H is really better. The overall number also hides weak groups. For H the weakest group is Kazakh degraded scans: 0.767 [0.591, 0.882]. For L the weakest group is Russian degraded scans: 0.567 [0.392, 0.726]. These intervals overlap too, so I am not sure which one is worse. But L is wrong on 13 of 30 invoices in its weak group, and that is a lot.

27% of the scans have a name and an IIN. With H, 542025 of these pages go abroad every year. The law No. 94-V covers the IIN. With H I know the scan goes to a data centre abroad, but I do not know who stores it and for how long, so we must ask the vendor. What the vendor says is not a compliance review. With L the scan stays on our server in Kazakhstan and the bank stores it. The bank only has to decide for how long.

Safeguards that we can check: first, check the arithmetic of every invoice and send the failed ones to a person; we can count them in a log. Second, no scan leaves our server; we can see this in the network rules. Third, delete scans after a fixed time; we can see this in the storage log. Fourth, every month test L on 30 degraded Russian invoices with a truth file.

L costs 5400 a year and H costs 17164, so L is cheaper by 11764. L also keeps personal data in Kazakhstan. The risk that stays is that L is weak on degraded Russian scans, so people must check more invoices.

## Part 3: Explain your own numbers

### Q1

Number: 1.51
Why (at most 40 words): Token ratio 3.23, byte ratio 2.14. Tokens grow 1.51 times more than bytes. So the premium is not only encoding (more bytes for Kazakh letters); the tokenizer adds more, because it has few Kazakh pieces.


### Q2

Number: 874
Why (at most 40 words): The chunk has about 920 e5 tokens, so the model reads only 512. The sentence starts at 874, so it is cut. A question matching only it cannot retrieve this chunk, and there is no error.

### Q3

Number: 66.6
Why (at most 40 words): Lower T makes the top token stronger and the others weaker. The ranking does not change. All tokens can still be sampled; only the chances change. Top-p would remove tokens.

### Q4

Number: 23.7
Why (at most 40 words): I left out arithmetic sequences and multiplication: they follow rules, not memorised facts. A system-prompt "never invent a fact" does not change the bound, because the model never learned those facts, and the prompt cannot add them.

### Q5

Number: 50895
Why (at most 40 words): By turn 29 the first message has been paid for 29 times, once on every turn. Prompt caching lets the old history be read at about one tenth of the price, so repeated tokens cost much less.

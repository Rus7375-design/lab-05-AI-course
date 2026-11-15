# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 had the biggest effect on the results. Before it, there were 16 correct answers, 4 wrong answers and 4 questions without an answer. After adding Layer 1, the number of correct answers increased to 18, and all questions received an answer. The model also started handling tool errors better. Instead of stopping when an error happened, it read the error and tried again. This helped q23 and q24 become correct. However, q21 and q22 became wrong because the model tried again but used the wrong region. Layer 2 did not change the results at all. Layer 3 also did not add any new correct answers. It only changed how the model handled uncertain answers: the number of wrong answers decreased from 6 to 4, while the number of unanswered questions increased from 0 to 2.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

I chose q18 (Jambyl). With Layer 3, the final answer was marked as UNVERIFIED. The model first used get_series and got the correct numbers: 16.1 for 2022 and 10.6 for 2025. The problem happened during the calculation. It used (16.1 - 10.6)/100 and got 0.055, but then wrote 0.55 as the final answer. The correct answer is 5.5 because we only need to subtract 10.6 from 16.1. The harness noticed that 0.55 was not in the tool results. But the model tried the same calculation again and made the same mistake. In the end, the harness marked the answer as UNVERIFIED. I would add a new tool called change that does the calculation automatically. This could help avoid mistakes. However, it would also make the tool description longer, require more code, and could cause new problems, for example, if the model gives the wrong arguments.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would choose the fixed workflow. It gave better results on the 15 questions in Part 1. It got 13 correct answers, while the agent got only 9. The workflow also needed fewer calls (agent 26), used fewer tokens (agent: 19,835) and was faster (agent: 48.4). The bare agent also made mistakes on ranking questions q08 and q11 because it sometimes chose the wrong end of the list. The workflow does not have this problem because the code always takes the correct end. Since these questions are the same every month, I think the workflow is the better choice. Of course, 15 questions is a small test, so we cannot be completely sure about the results. But the difference in speed and cost is quite big. I would use the agent for questions that change every time or need several steps, for example, calculating the percentage change between two years.

---

I wrote this file myself, without any AI tool.

# Online Evaluation Tools: Comparison
### Llumino, Task 1 Comparison

| # | What we need | Braintrust | AWS AgentCore Evaluations | LangSmith | Langfuse | Arize AX | Azure AI Foundry |
|---|---|---|---|---|---|---|---|
| 1 | **Works with agents running anywhere**, not just on one cloud | **Yes** works with any cloud and any framework | **No** it can only read agents running on AWS. It cannot see the others | Yes works anywhere | Yes works anywhere | Yes works anywhere | **No** it can only see agents running on Azure |
| 2 | **We control where our data is stored** | **Yes** our data stays in our own cloud account. Their servers never see it. We can also run the whole thing ourselves. Has SOC 2, HIPAA and GDPR | Partial the data stays in our AWS account, but only works on AWS | **No** their cloud only. Our data has to leave our network | **Yes** we run it fully on our own servers, and it is free to use | Partial the free version cannot do online checks at all. Online checks only come with the paid hosted version | Partial Azure only |
| 3 | **Grades real conversations as they happen** | Yes grades in the background, users notice no delay | Yes grades live traffic all the time | Yes grades live conversations | Yes grades what is already recorded, so it never slows the app down | Yes | Yes grades almost in real time |
| 4 | **We choose how many conversations to grade** | Yes about 1 in 100 up to 1 in 10 for busy apps, more for important ones | Yes anywhere from 1 in 10,000 up to all of them. Default is 1 in 10 | Yes | Yes they suggest grading 5 to 10 out of every 100 | Yes | Yes |
| 5 | **Can block a bad release** before it ships | Yes checks every code change and shows what got better and what got worse | Yes can be run as part of the release process | Yes | Partial it can send a warning, but it cannot stop a release by itself | Partial | Partial |
| 6 | **Keeps test sets saved and versioned**, so old tests still work later | Yes saves test sets, prompts and scoring rules with version history | Yes once a grader is in use it is locked, so nobody can quietly change it. This is the strongest control of all six | Yes | Partial we would have to build this part ourselves | Yes | Partial |
| 7 | **Checks if the agent used its tools correctly** and finished the job | Yes | Yes has ready-made checks for goal completion and tool use | Yes | Partial | Yes | Yes |
| 8 | **Ready-made quality and safety checks** | Yes checks for accuracy, relevance and safety | Yes 13 ready-made checks, but we cannot change how they work | Partial | Partial has fewer ready-made ones | Yes | Yes |
| 9 | **We can add our own checks** | Yes | Yes | Yes | Yes | Yes | Yes |
| 10 | **We can pick exactly what gets graded** | Yes can grade a whole conversation or just one step inside it | Partial can filter which conversations, but cannot pick a single step | Yes can also go back and grade old conversations, so a new check has history right away | Yes can grade a whole conversation or single steps | Yes | Partial |
| 11 | **We can check that the AI grader is actually right** | Yes people can review and correct scores | Partial no built-in way to do this | **Yes** the only tool that compares the AI grader against human scores and tracks how closely they agree | Partial | Yes | Partial |
| 12 | **Can send results to ModelAdvisor** | Partial we still have to build the connection | Partial we still have to build the connection | Partial | Yes we own the database, so we can read it directly | Partial | Partial |
| 13 | **How we pay** | Custom price, we need a quote for our volume | We pay per conversation graded | Per user or per conversation | Free to use, we only pay for the AI grader calls | Paid, hosted by them | Pay as we use it on Azure |
| 14 | **How hard to leave later** | Partial our test sets sit in their system | No very hard, we would be tied to AWS | Partial | **Yes** easiest, the data is already ours | Partial | No very hard, we would be tied to Azure |

---

## Which tool, and why

| Tool | Build or buy | Our call | Why | Pick this if |
|---|---|---|---|---|
| **Braintrust** | Buy | **Keep** | It is the only tool that does both of the two things that matter most. It works with agents running anywhere (row 1), and our data still stays in our own cloud account (row 2). We also get release blocking and version control on day one, with no building work (rows 5 and 6). | Our agents run on more than one cloud, and we want this working quickly. |
| **AWS AgentCore Evaluations** | Buy, we already have it | **Keep, but only if we are fully on AWS** | It cannot see agents that run outside AWS (row 1), so it can only cover part of our agents. But where it does work, it has the best control of any tool (row 6): once a check is switched on, nobody can quietly change it. That makes the standard real instead of just written in a document. | All our agents run on AWS and will keep running there. If not, it becomes one piece under Llumino instead of the main system. |
| **Langfuse, running on our own servers** | **Build** | **Keep** | This is the best option if we want full control. Our data never leaves (row 2), we can read results straight out of the database for ModelAdvisor (row 12), and it is the easiest to walk away from later (row 14). The trade-off is that we build the release blocking and version control ourselves, and then we own that code forever. | Our data is not allowed to leave our network, or control matters more to us than speed. |
| LangSmith | Buy | Drop | Our data would have to leave our network, which is likely a hard no (row 2). Worth knowing: it is the only tool that checks whether the AI grader is actually correct (row 11). That is the real answer to why teams do not trust these scores. If we do not buy it, we have to build that process ourselves. | Only if it turns out our data is allowed to leave after all. |
| Arize AX | Buy | Drop | Its free version cannot grade live conversations at all. That only comes with the paid hosted version, so the free path does not give us what we need (row 2). | Only if we decide to standardise on OpenTelemetry and are happy with them hosting it. |
| Azure AI Foundry | Buy | Drop | Same problem as AgentCore, it only sees agents on one cloud (row 1), and that cloud is not the one we appear to be on. | Only if we turn out to be an Azure shop. |

**Two questions decide this.** First, do all our agents run on AWS, and will they keep running there? If the answer is no, AgentCore cannot be the main system. Second, is our conversation data allowed to leave our network? If the answer is no, LangSmith is out straight away, and Braintrust only works if we use their setup where our data stays in our own account.

### If we choose AgentCore over Braintrust, why

We would pick AgentCore because it is already ours and it makes the standard real. There is no new vendor to bring in, no contract to negotiate and no security review to pass, because it is part of the AWS account we already use, so our conversation data never leaves our own network at any point. It comes with 13 ready-made checks for quality, safety, task completion and tool use, and it works with the agent frameworks our teams already build on. The strongest reason is control: once a check is switched on, AWS locks it, so nobody can quietly change how an agent is being graded, and permissions decide who is allowed to set up checks at all. That means Llumino would be a standard the platform enforces on its own, instead of a rule we write down and then have to chase people about. We would choose this only if all our agents run on AWS and will keep running there, and we would be accepting two real costs: it cannot see any agent outside AWS, and it would be hard to move away from later.

### If we choose Braintrust over AgentCore, why

We would pick Braintrust because it can cover every agent we have, not just some of them. AgentCore can only read agents running on AWS, so the moment any team builds somewhere else, we are back to having no single view of quality, which is exactly the problem Llumino is meant to solve. Braintrust works with any cloud and any framework, so one set of scores covers the whole company and the numbers can actually be compared across teams. Importantly, choosing it does not mean handing our data to an outside company: their setup keeps all our conversations and test sets inside our own cloud account, and their servers never see them. On top of that we get things that help adoption straight away, such as checking every code change and showing exactly what got better and what got worse, grading one single step inside a conversation instead of the whole thing, and letting people review and correct the scores by hand. We would choose this when our agents are spread across more than one place and we want something working soon, and we would be accepting a new vendor, a price we still need quoted, and test sets that sit in their system if we ever leave.

---

## Sources

- Braintrust: [Score production traces](https://www.braintrust.dev/docs/evaluate/score-online), [Online scoring](https://www.braintrust.dev/foundations/online-scoring), [Hybrid deployment](https://www.braintrust.dev/blog/hybrid-deployment), [Self-hosting](https://www.braintrust.dev/docs/admin/self-hosting)
- AWS: [AgentCore Evaluations](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/evaluations.html), [Online evaluation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/online-evaluations.html), [Create online evaluation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/create-online-evaluations.html), [Built-in evaluators](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/built-in-evaluators-overview.html)
- LangSmith: [LLM-as-a-judge online evaluators](https://docs.langchain.com/langsmith/online-evaluations-llm-as-judge), [Judge calibration](https://www.langchain.com/resources/llm-as-a-judge)
- Langfuse: [LLM-as-a-Judge](https://langfuse.com/docs/evaluation/evaluation-methods/llm-as-a-judge), [Evaluation overview](https://langfuse.com/docs/evaluation/overview)
- Arize: [AX or Phoenix decision guide](https://arize.com/resources/arize-ax-or-phoenix/)
- Microsoft: [Continuous evaluation for agents](https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/continuous-evaluation-agents?view=foundry-classic)

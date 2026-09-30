## This project tests OpenCode memory search CLI that has been implemented

I'll ask you questions about past agent sessions, you'll provide short, concise answers.

Then you may be given the correct answer. With the correct answer, we will build up labels
of relevant / irrelevant documents (agent transcripts) that help answer the question (or are 
misleading)

## Searching memories

You should use search_opencode_history to search past OpenCode agent sessions for relevant information

Be sure to run --help to get information on the options on how to filter information and select the best 
results.

Read [docs/search_tool.md](docs/search_tool.md) for more information on how to use the search tool.

Each search result is a transcript
- A human prompt
- A list of agent responses and tool calls

It's part of a *session* - basically the session is everything that happened with that chat context. 

A search result can also be a "system prompt" use case that starts a session

## Diagnosing basic functionality

You may be asked to run the search tool, get different bells and whistles, etc

## Gathering / labeling evidence

Destination file: judgments.csv

We want to turn the retrieval testing into a traditional IR task of queries <-> document relevance.

After you make an attempt to gather evidence, you may be presented with the correct answer by the user. Once you get
correct answer from the user, then you should search again, finding where that evidence would have been found.

This helps you find false-positives, true-positives, etc. In particular, confusing / misleading evidence can be helpful
in optimizing retrieval.

When you're given a question / prompt from the user, you should label the transcripts returned in retrieval as following:

user_question, grade, classification_code, transcript_id, comment

user_question: question provided to you, the agent, with some modification allowed so it can be understood stanadlone
grade (numerical grade, as below)
classification_code (one of I, M, H, C)
transcript_id (the id of the transcript as retrieved from the tool)
comment (optional, can be used to explain why you gave the grade)


0. Irrelevant (classification code: I) - has irrelevant evidence
0. Misleading (classification code: M) - has misleading evidence: IE it may help YOU the agent assume incorrect facts about history
1. Helpful (classification code: H) - guides to the correct answer, but doesn't contain all the info
2. Correct (classification code: C) - has correct evidence


## Functional / tool deficiencies

Destination file: functionality.csv

In a seperate file, we need to know functional deficiencies in the tool, the information its able to retrieve, and specifically what its not able to retrieve

This is more like a usage log, on a specific date, a given user query was serviced, we observed this functionality made it hard / different, we called the tool N times

We want to track a couple of kinds of 'functional' problems

### Functional

- Is the CLI lacking features that would help filter to the right context?
- What would help you solve this query?

### Missing data

- Is there a type of data not being retrieved that would help you solve this query? (ie project artifacts, etc)

### Format

The format is simple CSV with the following columns:

timestamp, user_question, deficiency_type, deficiency_description, num_tool_calls

deficiency_type a single letter code as follows:

- F - Functional deficiency
- M - Missing type of data
- O - Other deficiency

With deficiency_description a narative description of the deficiency, and how it would help you solve the query.

num_tool_calls is how many times the CLI was called as a proxy for output tokens


## Python scripts, etc to help

You can bootstrap this directory as a python repo, with uv, and create any scripts needed in a scripts/ folder.

It would be helpful to confirm CSV can be loaded with pandas, for example

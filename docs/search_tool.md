## Instructions for search_opencode_history

The search-tool (search-opencode-history) is a tool allowing searching of historical, user interactions with opencode 
with specific metadata

The filters in the tool allow the caller to select / search historical traces at different levels of granularity

The query allows you to formulate queries to he corpus of historical transcripts. Think about what makes good search queries.

### What are the documents you can search?

We index individual *transcripts* as the primary document.

A transcript is a single user message, the agents responses, and the intermediate tool calls / their outputs as a single piece of text.

Transcripts truncate tool call / tool output when they exceed a certain size. We also index a transcript with system prompt information at the start of every session.

The attributes on the document allow filtering depending on the level of granularity desired:

- project_path - the path to the project being worked on
- session_id - an agent session (ie one ongoing context window being worked on / chatted with, usually to complete a specific task)
- prompt_id - the id of the individual prompt / transcript
- prompt_ordinal - the ordinal of the prompt in the session (ie 1, 2, 3, etc). You can use this to filter to perhaps the previous prompt to get more context.
- id - the doc_id, a unique identifier for this document in turbopuffer
- transcript - the transcript (though with certain parts truncated / summarized). This is what's embedded or indexed.
- transcript_full - the transcript, not truncated, but only returned when --inspect called
- is_system_prompt - whether this indexed prompted is a system prompt (then the transcript is the AGENTS.md) (this can be selected via --system-metadata)

### How the search tool lets you search this

The search tool exposes many of the attributes for filtering the result set, ie

`project_path`, `session_id`, 

To filter to just view system / project level metadata, use --system-metadata

In addition, to get the full, unadulterated transcript for a prompt, the user can call

--inspect <id>

Additionally a query can be passed unless the selection chooses a single prompt. In which case the query is ignored.

### Unsure what to do, search system prompts first

If you just want a lay of the land, search system prompts. Set --system-metadata and search, that gives you a diverse overview.

Then zero-in to a specific project to explore

Then if needed, find specific sessions to filter into, and from there go through prompts as needed, even using --inspect to get the full transcript
of tool call data


### Use `jq` and `grep` to further search, if available 

The output of the search tool is JSON, so users can use `jq` to further filter the results, or `grep` to search for specific strings in the transcript.

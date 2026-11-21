# Blog Writing Crew: a multi-agent AI content pipeline with CrewAI

Give it a topic, and four AI agents research it, write a blog post, edit it to publication quality, and turn it into ready-to-post social media content.

Built with **Python**, **[CrewAI](https://crewai.com)** and the **Google Gemini API**.

## How it works

The crew runs as a **sequential pipeline**. Each agent gets the previous agent's output as its input.

```
 topic
   │
   ▼
┌────────────┐   research brief   ┌──────────┐   draft post   ┌──────────┐   final post   ┌──────────────────────┐
│ Researcher │ ─────────────────▶ │  Writer  │ ─────────────▶ │  Editor  │ ─────────────▶ │ Social Media Manager │
└────────────┘                    └──────────┘                └──────────┘                └──────────────────────┘
                                                                    │                                │
                                                                    ▼                                ▼
                                                           output/blog_post.md            output/social_posts.md
```

| Agent | Job | Output |
|---|---|---|
| **Research Analyst** | Finds key facts, trends and interesting angles on the topic | A research brief with 8–10 key points |
| **Blog Writer** | Turns the research into an engaging post with an intro, 3–4 sections and a conclusion | An 800–1,000 word draft in Markdown |
| **Senior Content Editor** | Polishes grammar, clarity, flow and tone | `output/blog_post.md` |
| **Social Media Content Strategist** | Repurposes the post for social platforms | `output/social_posts.md`: a 5–7 tweet X (Twitter) thread and a LinkedIn post |

## What I learned

- **Agents are configured, not hard-coded.** Each agent's role, goal and backstory live in `config/agents.yaml`, and each task's description and expected output live in `config/tasks.yaml`. `crew.py` wires them together, and the method names must match the YAML keys.
- **The backstory changes the output.** Giving the social media agent a "Gen Z social media native" backstory made the posts noticeably shorter and punchier, with more emojis and hooks.
- **Extending a crew is additive.** I added the Social Media Manager as a fourth agent without changing the original three: one new agent in YAML, one new task in YAML, and two decorated methods in `crew.py`.
- **Only the tasks that produce a deliverable write a file.** Intermediate results pass straight from one agent to the next.

## Sample output

The topic for the sample run was *"Machine Learning for Dummies"*.

- [`output/blog_post.md`](output/blog_post.md): the edited blog post
- [`output/social_posts.md`](output/social_posts.md): the X thread and LinkedIn post
- [`output/Zav_blog_post_01.md`](output/Zav_blog_post_01.md): a second run, on breaking into tech without a computer science degree

## Project structure

```
blog_writing_crew/
├── src/blog_writing_crew/
│   ├── config/
│   │   ├── agents.yaml   # roles, goals and backstories of the 4 agents
│   │   └── tasks.yaml    # task descriptions, expected outputs, output files
│   ├── crew.py           # wires agents and tasks into a sequential crew
│   └── main.py           # sets the topic and starts the crew
├── output/               # generated blog posts and social posts
├── pyproject.toml
└── uv.lock
```

## Run it yourself

**You need:** Python 3.10–3.13, [uv](https://docs.astral.sh/uv/), and a [Google Gemini API key](https://aistudio.google.com/apikey).

1. Install the CrewAI CLI and the project's dependencies:
   ```bash
   pip install uv
   uv tool install crewai
   crewai install
   ```
2. Create a `.env` file in the project folder. It is git-ignored, so your key stays private:
   ```
   MODEL=gemini/gemini-3.5-flash-lite
   GEMINI_API_KEY=your-key-here
   ```
3. Set your topic in `src/blog_writing_crew/main.py`:
   ```python
   inputs = {'topic': 'Your topic here', ...}
   ```
4. Run the crew:
   ```bash
   crewai run
   ```

The results are saved in the `output/` folder.

## About

Built by **Zheng Feng Goh** as part of a [NextWork](https://nextwork.ai) project. Read the full build write-up [on NextWork](https://nextwork.ai/overjoyed_lavender_happy_mouse/docs/99b558af-381c-5c9e-afef-a2d5e19992c0).

- LinkedIn: [linkedin.com/in/zheng-feng-goh](https://www.linkedin.com/in/zheng-feng-goh)
- GitHub: [github.com/ZavierZF](https://github.com/ZavierZF)

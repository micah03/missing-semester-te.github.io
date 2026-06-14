---
layout: lecture
title: "Agentic Coding"
description: >
  సాఫ్ట్‌వేర్ అభివృద్ధి పనుల కోసం AI కోడింగ్ ఏజెంట్లను సమర్థవంతంగా ఉపయోగించడం నేర్చుకోండి.
thumbnail: /static/assets/thumbnails/2026/lec7.png
date: 2026-01-21
ready: true
video:
  aspect: 56.25
  id: sTdz6PZoAnw
---

కోడింగ్ ఏజెంట్లు అనేవి ఫైళ్లను చదవడం/రాయడం, వెబ్ శోధన చేయడం, షెల్ కమాండ్లను అమలు చేయడం వంటి టూల్స్‌కు ప్రాప్యత కలిగిన సంభాషణాత్మక AI మోడల్స్. ఇవి IDE లోపల గానీ, లేదా స్వతంత్ర కమాండ్-లైన్ లేదా GUI టూల్స్ రూపంలో గానీ పనిచేస్తాయి. కోడింగ్ ఏజెంట్లు అత్యంత స్వయంప్రతిపత్తి కలిగిన మరియు శక్తివంతమైన టూల్స్‌గా ఉండి, విభిన్న రకాల ఉపయోగ సందర్భాలను సాధ్యమయ్యేలా చేస్తాయి.

ఈ ఉపన్యాసం, [Development Environment and Tools](/2026/development-environment/) ఉపన్యాసంలో పరిచయం చేసిన AI-ఆధారిత అభివృద్ధి అంశాలపై ఆధారపడి ముందుకు సాగుతుంది. ఒక చిన్న డెమోగా, [AI-powered development](/2026/development-environment/#ai-powered-development) విభాగంలో ఉపయోగించిన ఉదాహరణను కొనసాగిద్దాం:

```python
from urllib.request import urlopen

def download_contents(url: str) -> str:
    with urlopen(url) as response:
        return response.read().decode('utf-8')

def extract(content: str) -> list[str]:
    import re
    pattern = r'\[.*?\]\((.*?)\)'
    return re.findall(pattern, content)

print(extract(download_contents("https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/development-environment.md")))
```

క్రింది పనిని కోడింగ్ ఏజెంట్‌కు సూచనగా (prompt) ఇవ్వవచ్చు:

```
Turn this into a proper command-line program, with argparse for argument parsing. Add type annotations, and make sure the program passes type checking.
```

ఏజెంట్ ముందుగా ఫైల్‌ను చదివి దాని పనితీరును అర్థం చేసుకుంటుంది. తర్వాత అవసరమైన మార్పులు చేసి, చివరగా టైప్ అనోటేషన్లు సరిగ్గా ఉన్నాయో లేదో నిర్ధారించడానికి టైప్ చెకర్‌ను అమలు చేస్తుంది. ఒకవేళ టైప్ చెకింగ్ విఫలమయ్యే విధంగా ఏదైనా పొరపాటు జరిగితే, సాధారణంగా అది సమస్యను గుర్తించి మళ్లీ ప్రయత్నిస్తుంది. అయితే ఇది సరళమైన పని కావడంతో అలాంటి పరిస్థితి ఎదురయ్యే అవకాశం తక్కువ.

కోడింగ్ ఏజెంట్లకు హానికరంగా మారే అవకాశమున్న టూల్స్‌ను కూడా ఉపయోగించే సామర్థ్యం ఉండటం వల్ల, డిఫాల్ట్‌గా ఏజెంట్ హార్నెస్‌లు టూల్ కాల్స్‌ను అమలు చేసే ముందు వినియోగదారుని నిర్ధారణ (confirmation) కోరుతాయి.


> కోడింగ్ ఏజెంట్ ఏదైనా పొరపాటు చేస్తే — ఉదాహరణకు, `mypy` బైనరీ నేరుగా `$PATH` లో అందుబాటులో ఉన్నప్పటికీ, ఏజెంట్ `python -m mypy` ను అమలు చేయడానికి ప్రయత్నిస్తే — దానికి టెక్స్ట్ రూపంలో ఫీడ్‌బ్యాక్ ఇచ్చి సరైన మార్గంలోకి తీసుకురావచ్చు.

కోడింగ్ ఏజెంట్లు బహుళ-దశల (multi-turn) పరస్పర చర్యలను మద్దతు ఇస్తాయి. అందువల్ల, ఏజెంట్‌తో ముందుకు-వెనక్కి సంభాషిస్తూ పనిని దశలవారీగా మెరుగుపరచుకోవచ్చు. ఒకవేళ ఏజెంట్ తప్పు దిశలో వెళ్తోందని అనిపిస్తే, మధ్యలోనే దాన్ని ఆపివేయవచ్చు కూడా.

దీనిని అర్థం చేసుకోవడానికి ఒక ఉపయోగకరమైన మానసిక నమూనా (mental model) ఏమిటంటే, మీరు ఒక ఇంటర్న్‌కు మేనేజర్‌గా ఉన్నారని ఊహించుకోవడం. ఇంటర్న్ సూక్ష్మ స్థాయి పనులను నిర్వహిస్తాడు, కానీ అతనికి మార్గదర్శకత్వం అవసరం ఉంటుంది. అప్పుడప్పుడు అతను తప్పులు చేయవచ్చు; అలాంటి సందర్భాల్లో మీరు వాటిని గుర్తించి సరిదిద్దాల్సి ఉంటుంది.

> మరింత స్పష్టమైన డెమో కోసం, తదుపరి సూచనగా ఏజెంట్‌ను రూపొందించిన స్క్రిప్ట్‌ను అమలు చేయమని అడగండి. వచ్చిన ఫలితాలను గమనించండి. తరువాత దానిలో ఒక మార్పు చేయమని చెప్పండి (ఉదాహరణకు, కేవలం absolute URLs మాత్రమే చేర్చేలా మార్చమని అడగండి).

# AI మోడల్స్ మరియు ఏజెంట్లు ఎలా పనిచేస్తాయి

ఆధునిక [Large Language Models (LLMs)](https://en.wikipedia.org/wiki/Large_language_model) మరియు agent harnesses వంటి మౌలిక సదుపాయాల అంతర్గత పనితీరును పూర్తిగా వివరించడం ఈ కోర్సు పరిధికి మించిన విషయం. అయితే, ఈ అత్యాధునిక సాంకేతికతను సమర్థవంతంగా _ఉపయోగించడానికి_ మరియు దాని పరిమితులను అర్థం చేసుకోవడానికి కొన్ని ముఖ్యమైన భావనలపై ఉన్నత-స్థాయి అవగాహన కలిగి ఉండటం ఉపయోగకరంగా ఉంటుంది.

LLMs ను, prompt strings (ఇన్‌పుట్లు) ఇచ్చినప్పుడు completion strings (అవుట్‌పుట్లు) వచ్చే సంభావ్యత పంపిణీ (probability distribution)ను నమూనా చేసే వ్యవస్థలుగా చూడవచ్చు. LLM inference (ఉదాహరణకు, మీరు ఒక conversational chat app లో ప్రశ్న ఇచ్చినప్పుడు జరిగే ప్రక్రియ) ఈ probability distribution నుండి _sample_ చేస్తుంది.

LLMs కు ఒక స్థిరమైన _context window_ ఉంటుంది. ఇది ఇన్‌పుట్ మరియు అవుట్‌పుట్ స్ట్రింగ్‌ల మొత్తం పొడవుకు అనుమతించబడే గరిష్ట పరిమితి.

{% comment %}
> గణిత సంకేతాలలో చెప్పాలంటే, LLM అనేది prompts $x$ ఇవ్వబడినప్పుడు completions $y$ యొక్క సంభావ్యత పంపిణీ (probability distribution) $\pi_\theta$ ను నమూనా చేస్తుంది. అనంతరం, మేము ఆ పంపిణీ నుండి ఒక నమూనాను (sample) తీసుకుంటాము: $\hat{y} \sim \pi_\theta(\cdot \mid x)$.
{% endcomment %}

AI టూల్స్, ఉదాహరణకు conversational chat applications మరియు coding agents, ఈ ప్రాథమిక భావనపై నిర్మించబడ్డాయి. బహుళ-దశల (multi-turn) పరస్పర చర్యల కోసం, chat applications మరియు agents turn markers ను ఉపయోగించి, ప్రతి కొత్త user prompt వచ్చినప్పుడు మొత్తం సంభాషణ చరిత్రను prompt string గా అందిస్తాయి. ప్రతి user prompt కు ఒకసారి LLM inference అమలవుతుంది.

Tool-calling agents విషయంలో, harness కొన్ని LLM outputs ను ఒక టూల్‌ను అమలు చేయాలనే అభ్యర్థనలుగా (requests) పరిగణిస్తుంది. అనంతరం, ఆ tool call ఫలితాలను prompt string లో భాగంగా తిరిగి మోడల్‌కు అందిస్తుంది. అందువల్ల, ప్రతి tool call మరియు response సమయంలో LLM inference మళ్లీ అమలవుతుంది.

Tool-calling agents వెనుక ఉన్న ప్రధాన భావనలను కేవలం [200 lines of code](https://www.mihaileric.com/The-Emperor-Has-No-Clothes/) లోనే అమలు చేయవచ్చు.

## గోప్యత (Privacy)

చాలా AI కోడింగ్ టూల్స్ తమ సాధారణ (standard) కాన్ఫిగరేషన్‌లలో మీ డేటాలో గణనీయమైన భాగాన్ని క్లౌడ్‌కు పంపిస్తాయి. కొన్ని సందర్భాల్లో harness మీ స్థానిక కంప్యూటర్‌లో (locally) అమలవుతుండగా, LLM inference మాత్రం క్లౌడ్‌లో జరుగుతుంది. మరికొన్ని సందర్భాల్లో, సాఫ్ట్‌వేర్‌లోని ఇంకా ఎక్కువ భాగం క్లౌడ్‌లోనే నడుస్తుంది. ఉదాహరణకు, సేవా ప్రదాత (service provider) మీ మొత్తం repository యొక్క కాపీతో పాటు AI టూల్‌తో మీరు చేసే అన్ని పరస్పర చర్యల (interactions) కాపీని కూడా పొందే అవకాశం ఉంది.

మంచి నాణ్యత కలిగిన open-source AI కోడింగ్ టూల్స్ మరియు open-source LLMs కూడా అందుబాటులో ఉన్నాయి (అయితే అవి proprietary models స్థాయికి ఇంకా పూర్తిగా చేరుకోలేదు). అయితే ప్రస్తుతం, చాలా మంది వినియోగదారులకు అత్యాధునిక (bleeding-edge) open LLMs ను స్థానికంగా (locally) అమలు చేయడం హార్డ్‌వేర్ పరిమితుల కారణంగా సాధ్యపడకపోవచ్చు.

# ఉపయోగ సందర్భాలు (Use Cases)

Coding agents అనేక రకాల పనుల్లో ఉపయోగపడతాయి. కొన్ని ఉదాహరణలు:

- **కొత్త features ను అమలు చేయడం (Implementing new features).** పై ఉదాహరణలో చూపినట్లుగా, మీరు ఒక coding agent ను ఉపయోగించి కొత్త feature ను implement చేయమని అడగవచ్చు. మంచి specification ఇవ్వడం అనేది ఒక కళ (art) లాంటిది. మీ input, agent మీరు కోరుకున్న పనిని చేయడానికి సరిపడా వివరణాత్మకంగా ఉండాలి (కనీసం సరైన దిశలో వెళ్లేలా ఉండాలి, తద్వారా మీరు తర్వాత iterate చేయగలరు). అయితే, మీరు స్వయంగా ఎక్కువ పని చేసినట్టుగా అయ్యేంత వివరంగా ఉండకూడదు. Test-Driven Development (TDD) ఇక్కడ చాలా ప్రభావవంతంగా ఉంటుంది: ముందుగా tests రాయండి (లేదా coding agent సహాయంతో tests రాయించండి), అవి మీ అవసరాలను సరిగ్గా ప్రతిబింబిస్తున్నాయో లేదో పరిశీలించండి, ఆ తర్వాత feature ను implement చేయమని agent ను అడగండి. Models నిరంతరం మెరుగుపడుతున్నందున, వాటి సామర్థ్యాలపై మీ అవగాహనను కూడా ఎప్పటికప్పుడు నవీకరించుకోవాలి.
    > మేము Claude Code ను ఉపయోగించి ఈ Tufte-style sidenotes ను [implement](https://github.com/missing-semester/missing-semester/pull/345) చేశాము.

{%- comment %}
ఇక్కడ demo చూపాల్సిన అవసరం లేదు, ఎందుకంటే lecture ప్రారంభంలోనే కొత్త feature ను జోడించే చిన్న demo ఉంది.
{% endcomment %}

- **లోపాలను సరిచేయడం (Fixing errors).** Compiler, linter, type checker లేదా tests నుండి errors వస్తే, వాటిని సరిచేయమని మీ agent ను అడగవచ్చు. ఉదాహరణకు, `"fix the issues with mypy"` వంటి prompt ఇవ్వవచ్చు. Coding models, feedback loop లో ఉన్నప్పుడు ప్రత్యేకంగా సమర్థవంతంగా పనిచేస్తాయి. కాబట్టి, సాధ్యమైనంతవరకు model స్వయంగా failing check ను run చేయగలిగే విధంగా setup చేయండి. అప్పుడు అది స్వతంత్రంగా (autonomously) iterate చేయగలదు. అది సాధ్యం కాకపోతే, మీరు feedback ను మాన్యువల్‌గా ఇవ్వవచ్చు.
    > missing-semester repository లోని commit [f552b55](https://github.com/missing-semester/missing-semester/commit/f552b5523462b22b8893a8404d2110c4e59613dd) పై, మేము Claude Code కు `"Review the agentic coding lecture for typos and grammatical issues"` అనే prompt ఇచ్చాము. తర్వాత అది గుర్తించిన సమస్యలను సరిచేయమని అడిగాము. ఆ మార్పులు [f1e1c41](https://github.com/missing-semester/missing-semester/commit/f1e1c417adba6b4149f7eef91ff5624de40dc637) commit లో చేర్చబడ్డాయి.

{%- comment %}
https://github.com/anishathalye/dotbot/commit/cef40c902ef0f52f484153413142b5154bbc5e99 లో ఉన్న bug ను coding agent తో fix చేసే demo చూపించండి.

Bug ను చూపించడానికి failing tests రాసి, తర్వాత agent ను fix చేయమని అడగండి. demo-bugfix branch లో సిద్ధంగా ఉంది.

Failing test ను run చేయడానికి:

    hatch test tests/test_cli.py::test_issue_357

Coding agent కు ఈ prompt ఇవ్వవచ్చు:

    There is a bug I wrote a failing test for, you can repro it with `hatch test tests/test_cli.py::test_issue_357`. Fix the bug.

మార్పులను commit చేయమని కూడా చెప్పండి.
{% endcomment %}

- **Refactoring.** Coding agents ను ఉపయోగించి code ను వివిధ రకాలుగా refactor చేయవచ్చు. ఇది ఒక method పేరు మార్చడం వంటి సులభమైన పనుల నుండి ([code intelligence](/2026/development-environment/#code-intelligence-and-language-servers) లో కూడా ఈ రకమైన refactoring కు మద్దతు ఉంది), functionality ను వేరే module గా విభజించడం వంటి క్లిష్టమైన పనుల వరకు ఉండవచ్చు.
    > మేము Claude Code ను ఉపయోగించి agentic coding ను ప్రత్యేక lecture గా [split](https://github.com/missing-semester/missing-semester/pull/344) చేశాము.

{%- comment %}
Missing Semester లో usage ను చూపించండి. అలాగే agent కొన్ని తప్పులు కూడా చేసిందని సూచించండి.
{% endcomment %}

- **Code Review.** Code ను review చేయమని కూడా coding agents ను అడగవచ్చు. ఉదాహరణకు, `"review my latest changes that are not yet committed"` వంటి సాధారణ సూచనలు ఇవ్వవచ్చు. మీరు ఒక Pull Request (PR) ను review చేయాలనుకుంటే, మీ coding agent web fetch ను support చేస్తే లేదా మీ వద్ద [GitHub CLI](https://cli.github.com/) వంటి command-line tools install చేసి ఉంటే, `"Review the pull request {link}"` అని మాత్రమే అడిగినా చాలాసార్లు agent మిగతా ప్రక్రియను స్వయంగా నిర్వహించగలదు.

{%- comment %}
Porcupine repository లో agent కు ఈ prompt ఇవ్వండి:

    Review this PR: https://github.com/anishathalye/porcupine/pull/39

{% endcomment %}

- **Code ను అర్థం చేసుకోవడం (Code understanding).** ఒక codebase గురించి coding agent ను ప్రశ్నలు అడగవచ్చు. ముఖ్యంగా కొత్తగా project లో చేరినప్పుడు (onboarding సమయంలో) ఇది చాలా ఉపయోగకరంగా ఉంటుంది.

{%- comment %}
missing-semester repository లో ప్రయత్నించడానికి కొన్ని prompts:

    How do I run this site locally?

    How are the social preview cards implemented?
{% endcomment %}

- **Shell గా ఉపయోగించడం (As a shell).** ఒక నిర్దిష్ట task ను పూర్తి చేయడానికి coding agent ను ప్రత్యేక tool ఉపయోగించమని అడగవచ్చు. దీంతో మీరు natural language ద్వారా shell commands ను invoke చేయవచ్చు. ఉదాహరణకు, `"use the find command to find all files older than 30 days"` లేదా `"use mogrify to resize all the jpgs to 50% of their original size"` వంటి prompts ఇవ్వవచ్చు.

{%- comment %}
Dotbot repository లో agent కు ఈ prompt ఇవ్వండి:

    Use the ag command to find all Python renaming imports

{% endcomment %}

- **Vibe Coding.** Agents ఇప్పుడు అంత శక్తివంతంగా మారాయి కాబట్టి, కొన్ని applications ను మీరు స్వయంగా ఒక్క line of code కూడా రాయకుండానే రూపొందించగలరు.

    > ఒక instructor పూర్తిగా vibe-coded చేసిన నిజ జీవిత project కు [ఇది ఒక ఉదాహరణ](https://github.com/cleanlab/office-presence-dashboard).

{%- comment %}
missing-semester repository లో agent కు ఈ prompt ఇవ్వండి:

    Make this site look retro.

{% endcomment %}

# అధునాతన Agents (Advanced Agents)

ఇక్కడ, coding agents యొక్క కొన్ని అధునాతన వినియోగ విధానాలు (advanced usage patterns) మరియు సామర్థ్యాల (capabilities) గురించి సంక్షిప్త అవలోకనం అందిస్తున్నాము.

- **మళ్లీ ఉపయోగించగల Prompts (Reusable Prompts).** మళ్లీ మళ్లీ ఉపయోగించగల prompts లేదా templates ను సృష్టించవచ్చు. ఉదాహరణకు, ఒక ప్రత్యేక విధానంలో code review చేయడానికి విపులమైన prompt రాసి, దాన్ని reusable prompt గా సేవ్ చేసుకోవచ్చు.

    > Agent tooling చాలా వేగంగా అభివృద్ధి చెందుతోంది. కొన్ని tools లో, standalone feature గా reusable prompts ఇప్పుడు deprecated అయ్యాయి. ఉదాహరణకు, Codex మరియు Claude Code లో, అవి [skills](https://code.claude.com/docs/en/skills) ద్వారా [subsumed](https://developers.openai.com/codex/custom-prompts) అయ్యాయి.

- **Parallel Agents.** Coding agents కొన్నిసార్లు నెమ్మదిగా పని చేస్తాయి. మీరు prompt ఇచ్చిన తర్వాత, ఒక సమస్యపై అవి పదుల నిమిషాల పాటు పని చేయవచ్చు. ఒకేసారి అనేక agent instances ను నడపవచ్చు. అవి ఒకే task పై పని చేయవచ్చు (LLMs stochastic కావడం వల్ల, ఒకే task ను అనేకసార్లు run చేసి ఉత్తమ ఫలితాన్ని ఎంచుకోవడం ఉపయోగకరంగా ఉంటుంది) లేదా వేర్వేరు tasks పై పని చేయవచ్చు (ఉదాహరణకు, పరస్పరం సంబంధం లేని రెండు features ను ఒకేసారి implement చేయడం).

    వేర్వేరు agents చేసిన మార్పులు ఒకదానితో ఒకటి ఘర్షణ పడకుండా ఉండేందుకు, [git worktrees](https://git-scm.com/docs/git-worktree) ను ఉపయోగించవచ్చు. దీనిని మేము [version control](/2026/version-control/) lecture లో చర్చిస్తాము.

- **MCPs.** MCP అంటే _Model Context Protocol_. ఇది మీ coding agents ను వివిధ tools తో అనుసంధానించడానికి ఉపయోగించే ఒక open protocol.

    ఉదాహరణకు, ఈ [Notion MCP server](https://github.com/makenotion/notion-mcp-server) ద్వారా మీ agent Notion documents ను చదవగలదు మరియు రాయగలదు. దీంతో `"read the spec linked in {Notion doc}, draft an implementation plan as a new page in Notion, and then implement a prototype"` వంటి workflows సాధ్యమవుతాయి.

    MCPs ను కనుగొనడానికి [Pulse](https://www.pulsemcp.com/servers) మరియు [Glama](https://glama.ai/mcp/servers) వంటి directories ను ఉపయోగించవచ్చు.

- **Context Management.** మేము [పైన](#how-ai-models-and-agents-work) పేర్కొన్నట్లుగా, coding agents కు ఆధారమైన LLMs కు పరిమితమైన _context window_ ఉంటుంది. Coding agents ను సమర్థవంతంగా ఉపయోగించాలంటే context ను సరిగ్గా నిర్వహించడం చాలా ముఖ్యమైనది.

    Agent కు అవసరమైన సమాచారం అందుబాటులో ఉండేలా చూసుకోవాలి. అయితే, అవసరం లేని context ను ఇవ్వకూడదు. లేకపోతే context window నిండిపోవచ్చు లేదా model పనితీరు తగ్గవచ్చు. (Context పరిమాణం పెరిగేకొద్దీ, context window overflow కాకపోయినా పనితీరు తగ్గే అవకాశం ఉంటుంది.)

    Agent harnesses కొంతవరకు context ను స్వయంచాలకంగా అందించి నిర్వహిస్తాయి. అయినప్పటికీ, గణనీయమైన నియంత్రణ (control) వినియోగదారుడి చేతుల్లోనే ఉంటుంది.

    - **Context Window ను Clear చేయడం.** ఇది అత్యంత ప్రాథమిక నియంత్రణ. చాలా coding agents context window ను clear చేయడం (అంటే కొత్త conversation ప్రారంభించడం) ను support చేస్తాయి. సంబంధం లేని కొత్త ప్రశ్నల కోసం ఇది చేయడం మంచిది.

    - **Conversation ను Rewind చేయడం.** కొన్ని coding agents conversation history లోని కొన్ని దశలను undo చేసే సౌకర్యాన్ని అందిస్తాయి. Agent ను వేరే దిశలో నడిపించడానికి follow-up message పంపడం కంటే, "undo" చేయడం సరైన సందర్భాల్లో context ను మరింత సమర్థవంతంగా నిర్వహించడంలో సహాయపడుతుంది.

{%- comment %}
త్వరిత demo కోసం ఒక ఉదాహరణను రూపొందించండి.
{% endcomment %}
    - **Compaction.** అపరిమిత (unbounded) పొడవు గల conversations ను నిర్వహించడానికి, coding agents context _compaction_ ను support చేస్తాయి. Conversation history చాలా పెద్దదిగా మారినప్పుడు, అవి స్వయంచాలకంగా ఒక LLM ను ఉపయోగించి conversation ప్రారంభ భాగం (prefix) యొక్క సారాంశాన్ని (summary) రూపొందిస్తాయి. ఆ తర్వాత అసలు conversation history స్థానంలో ఆ summary ను ఉంచుతాయి. కొన్ని agents, అవసరమైనప్పుడు compaction ను మాన్యువల్‌గా invoke చేసే నియంత్రణను కూడా వినియోగదారునికి అందిస్తాయి.

{%- comment %}
Claude Code లో `/compact` ను చూపించండి. పూర్తి summary ను కూడా ప్రదర్శించండి.
{% endcomment %}

    - **llms.txt.** `/llms.txt` ఫైల్ అనేది inference సమయంలో LLMs ఉపయోగించడానికి ఉద్దేశించిన document కోసం ప్రతిపాదించబడిన ఒక [ప్రామాణిక (standard)](https://llmstxt.org/) స్థానం.

      Products (ఉదాహరణకు, [cursor.com/llms.txt](https://cursor.com/llms.txt)), software libraries (ఉదాహరణకు, [ai.pydantic.dev/llms.txt](https://ai.pydantic.dev/llms.txt)), మరియు APIs (ఉదాహరణకు, [apify.com/llms.txt](https://apify.com/llms.txt)) తమ స్వంత `llms.txt` ఫైళ్లను కలిగి ఉండవచ్చు. ఇవి development సమయంలో చాలా ఉపయోగకరంగా ఉంటాయి.

      ఇటువంటి documents, HTML pages తో పోలిస్తే ప్రతి token కు ఎక్కువ సమాచారం (information density) కలిగి ఉంటాయి. అందువల్ల, coding agent ను ఒక HTML page ను fetch చేసి చదవమని అడగడం కంటే ఇవి context పరంగా మరింత సమర్థవంతమైనవి (context-efficient).

      ముఖ్యంగా, మీరు ఉపయోగించాలనుకుంటున్న dependency గురించి coding agent కు built-in knowledge లేనప్పుడు (ఉదాహరణకు, ఆ dependency LLM యొక్క knowledge cutoff తర్వాత విడుదలైనట్లయితే), ఈ విధమైన external documentation చాలా ఉపయోగపడుతుంది.

{%- comment %}
ఖాళీ repository లో (Desktop లేదా ఇతర self-contained location లో, `git init` run చేసిన తర్వాత) side-by-side comparison చూపించండి:

    Write a single-file Python program example in demo.py using semlib to sort "Ilya Sutskever", "Soumith Chintala", and "Donald Knuth" in terms of their fame as AI researchers.

    Write a single-file Python program example in demo.py using semlib to sort "Ilya Sutskever", "Soumith Chintala", and "Donald Knuth" in terms of their fame as AI researchers. See https://semlib.anish.io/llms.txt. Follow links to Markdown versions of any pages linked in llms.txt files.

Agent ఇది default గా ఎందుకు చేయదో ఖచ్చితంగా తెలియదు. సాధారణంగా మీరు ఆ చివరి వాక్యాన్ని `CLAUDE.md` ఫైల్‌లో ఉంచవచ్చు.
{% endcomment %}
    - **AGENTS.md.** చాలా coding agents, coding agents కోసం రూపొందించిన README లాంటి ఫైల్‌గా [AGENTS.md](https://agents.md/) లేదా దానికి సమానమైన ఫైల్‌ను support చేస్తాయి (ఉదాహరణకు, Claude Code `CLAUDE.md` కోసం చూస్తుంది).

      Agent ప్రారంభమైనప్పుడు, అది `AGENTS.md` లోని మొత్తం content ను context లో ముందుగానే (pre-fill) చేర్చుతుంది. దీనిని ఉపయోగించి, అన్ని sessions లో సాధారణంగా వర్తించే సూచనలను agent కు ఇవ్వవచ్చు.

      ఉదాహరణలు:

      - Code changes చేసిన తర్వాత ఎల్లప్పుడూ type-checker ను run చేయమని సూచించడం
      - Unit tests ను ఎలా run చేయాలో వివరించడం
      - Agent browse చేయగల third-party documentation links ను అందించడం

      కొన్ని coding agents ఈ ఫైల్‌ను స్వయంచాలకంగా రూపొందించగలవు. ఉదాహరణకు, Claude Code లోని `/init` command దీనికి ఉపయోగపడుతుంది.

      వాస్తవ ప్రపంచంలో ఉపయోగించే `AGENTS.md` ఉదాహరణ కోసం [ఇక్కడ](https://github.com/pydantic/pydantic-ai/blob/main/CLAUDE.md) చూడండి.

{%- comment %}
Dotbot ఉదాహరణ:

`CLAUDE.md` లో `@DEVELOPMENT.md` ను include చేసి, Python code లో ఏ మార్పులు చేసినా type checker మరియు code formatter ను తప్పనిసరిగా run చేయాలని పేర్కొనండి.

Demonstration కోసం, master branch పై ఈ prompt ఉపయోగించండి:

    Remove the "--version" command-line flag.

ఇది త్వరగా పూర్తయ్యే పని కాబట్టి demo కు అనుకూలంగా ఉంటుంది.
{% endcomment %}
    - **Skills.** `AGENTS.md` లోని content మొత్తం, ఎలాంటి ఎంపిక లేకుండా (in its entirety) agent యొక్క context window లో ఎల్లప్పుడూ load అవుతుంది.

      _Skills_ అనేవి context bloat ను తగ్గించడానికి ఒక అదనపు abstraction layer ను అందిస్తాయి. ఇందులో, మీరు agent కు skills జాబితాను వాటి వివరణలతో (descriptions) అందిస్తారు. అవసరమైనప్పుడు agent ఒక skill ను "open" చేసి (అంటే, ఆ skill యొక్క content ను తన context window లో load చేసి) ఉపయోగించవచ్చు.

      ఈ విధానం వల్ల అవసరం లేని సమాచారం ఎప్పుడూ context లో ఉండదు, తద్వారా context window మరింత సమర్థవంతంగా ఉపయోగించబడుతుంది.

    - **Subagents.** కొన్ని coding agents, task-specific workflows కోసం ప్రత్యేకమైన agents అయిన _subagents_ ను నిర్వచించే అవకాశం ఇస్తాయి.

      Top-level coding agent, ఒక నిర్దిష్ట task ను పూర్తి చేయడానికి subagent ను invoke చేయగలదు. దీని వల్ల top-level agent మరియు subagent రెండూ తమ context ను మరింత సమర్థవంతంగా నిర్వహించగలవు.

      - Top-level agent యొక్క context, subagent చూసే మొత్తం సమాచారంతో నిండిపోదు.
      - Subagent తన task కు అవసరమైన context మాత్రమే పొందుతుంది.

      ఉదాహరణకు, కొన్ని coding agents లో web research ను ఒక subagent గా అమలు చేస్తారు:

      1. Top-level agent, subagent కు ఒక query ఇస్తుంది.
      2. Subagent web search నిర్వహిస్తుంది.
      3. అవసరమైన web pages ను retrieve చేస్తుంది.
      4. వాటిని విశ్లేషిస్తుంది (analyze).
      5. చివరగా, query కు సమాధానాన్ని top-level agent కు అందిస్తుంది.

      ఈ విధానం వల్ల:

      - Top-level agent యొక్క context, retrieve చేసిన అన్ని web pages యొక్క పూర్తి content తో నిండిపోదు.
      - Subagent యొక్క context లో, top-level agent యొక్క మిగతా conversation history ఉండదు.

      ఫలితంగా, రెండు agents కూడా తమ తమ పనిపై మరింత దృష్టి పెట్టగలవు.

అధునాతన features లో చాలా వరకు (ఉదాహరణకు, skills లేదా subagents) prompts రాయడం అవసరం అవుతుంది. వీటిని ప్రారంభించడానికి LLMs సహాయాన్ని కూడా ఉపయోగించవచ్చు.

కొన్ని coding agents ఈ ప్రక్రియకు built-in support కూడా అందిస్తాయి. ఉదాహరణకు, Claude Code లో చిన్న prompt ఆధారంగా ఒక subagent ను స్వయంచాలకంగా రూపొందించవచ్చు (`/agents` command ను invoke చేసి కొత్త agent ను సృష్టించండి).

ఉదాహరణకు, ఈ prompt ను ఉపయోగించి ఒక subagent ను రూపొందించవచ్చు:

```text
A Python code checking agent that uses `mypy` and `ruff` to type-check, lint, and format *check* any files that have been modified from the last git commit.
```

ఆ తర్వాత, top-level agent ను ఉపయోగించి subagent ను స్పష్టంగా invoke చేయవచ్చు. ఉదాహరణకు:

```text
use the code checker subagent
```

అలాగే, కొన్ని సందర్భాల్లో top-level agent ను సరైన సమయంలో subagent ను స్వయంచాలకంగా invoke చేసేలా కూడా configure చేయవచ్చు. ఉదాహరణకు, ఏదైనా Python file ను modify చేసిన తర్వాత code checker subagent ను ఆటోమేటిక్‌గా నడపవచ్చు.

# జాగ్రత్తగా గమనించాల్సిన విషయాలు (What to Watch Out For)

AI tools తప్పులు చేయగలవు. ఇవి LLMs (Large Language Models) పై ఆధారపడి నిర్మించబడ్డాయి. LLMs అనేవి మూలంగా probabilistic next-token prediction models మాత్రమే. అవి మనుషుల మాదిరిగా "తెలివైనవి" (intelligent) కావు.

AI రూపొందించిన output ను correctness మరియు security bugs పరంగా తప్పనిసరిగా సమీక్షించాలి (review చేయాలి). కొన్ని సందర్భాల్లో, AI రాసిన code ను ధృవీకరించడం (verify చేయడం), ఆ code ను మీరు స్వయంగా రాయడం కంటే కూడా కష్టంగా ఉండవచ్చు. ముఖ్యమైన (critical) code కోసం, అవసరమైతే code ను చేతితో (manually) రాయడాన్ని పరిగణించండి.

AI కొన్నిసార్లు తప్పు దారిలో (rabbit hole) వెళ్లి, తప్పు నిర్ణయాలను సరైనవిగా చూపించడానికి ప్రయత్నించవచ్చు. Debugging సమయంలో ఇటువంటి spirals గురించి అప్రమత్తంగా ఉండాలి.

AI ను పూర్తిగా ఆధారపడే సాధనంగా (crutch) ఉపయోగించవద్దు. అతిగా ఆధారపడడం (overreliance) లేదా విషయంపై ఉపరితల (shallow) అవగాహనతో సరిపెట్టుకోవడం పట్ల జాగ్రత్తగా ఉండాలి.

ఇప్పటికీ AI చేయలేని programming tasks యొక్క విస్తృత శ్రేణి ఉంది. అందువల్ల, computational thinking ఇప్పటికీ ఎంతో విలువైన నైపుణ్యంగా మిగిలి ఉంది.

# సిఫార్సు చేయబడిన Software (Recommended Software)

అనేక IDEs మరియు AI coding extensions లో ఇప్పటికే coding agents అందుబాటులో ఉన్నాయి (దీనికి సంబంధించిన సిఫార్సులను [development environment lecture](/2026/development-environment/) లో చూడవచ్చు).

ఇతర ప్రముఖ coding agents:

- Anthropic యొక్క [Claude Code](https://www.claude.com/product/claude-code)
- OpenAI యొక్క [Codex](https://openai.com/codex/)
- Open-source agents, ఉదాహరణకు [opencode](https://github.com/anomalyco/opencode)

ఇవి code generation, refactoring, debugging, code review మరియు ఇతర development workflows ను మరింత సమర్థవంతంగా నిర్వహించడంలో సహాయపడతాయి.

# అభ్యాసాలు (Exercises)

1. ఒకే programming task ను నాలుగు సార్లు పూర్తి చేసి, చేతితో coding చేయడం, AI autocomplete ఉపయోగించడం, inline chat ఉపయోగించడం, మరియు agents ఉపయోగించడం మధ్య అనుభవాన్ని పోల్చండి.

   ఇందుకు ఉత్తమ ఎంపిక, మీరు ఇప్పటికే పని చేస్తున్న project లోని ఒక చిన్న feature. మరిన్ని ఆలోచనలు కావాలంటే, GitHub లోని open-source projects లో "good first issue" తరహా tasks ను పూర్తి చేయడం, లేదా [Advent of Code](https://adventofcode.com/) / [LeetCode](https://leetcode.com/) సమస్యలను పరిష్కరించడం పరిగణించవచ్చు.

2. పరిచయం లేని (unfamiliar) codebase ను అర్థం చేసుకోవడానికి AI coding agent ను ఉపయోగించండి.

   మీరు నిజంగా ఆసక్తి ఉన్న project లో bug ను debug చేయడం లేదా కొత్త feature ను జోడించడం వంటి సందర్భాల్లో ఇది అత్యంత ఉపయోగకరంగా ఉంటుంది.

   అలాంటి project మీకు గుర్తుకు రాకపోతే, [opencode](https://github.com/anomalyco/opencode) agent లో security-related features ఎలా పనిచేస్తాయో AI agent సహాయంతో అర్థం చేసుకోవడానికి ప్రయత్నించండి.

3. మొదటి నుండి (from scratch) ఒక చిన్న app ను vibe code చేయండి.

   ఈ exercise లో మీరు స్వయంగా ఒక్క line of code కూడా రాయకూడదు.

4. మీకు ఇష్టమైన coding agent కోసం క్రింది వాటిని సృష్టించి పరీక్షించండి:

   - `AGENTS.md` (లేదా మీ agent కు సమానమైన ఫైల్, ఉదాహరణకు `CLAUDE.md`)
   - ఒక skill (ఉదాహరణకు, [Claude Code skill](https://code.claude.com/docs/en/skills) లేదా [Codex skill](https://developers.openai.com/codex/skills/))
   - ఒక subagent (ఉదాహరణకు, [Claude Code subagent](https://code.claude.com/docs/en/sub-agents))

   వీటిలో ప్రతి ఒక్కదాన్ని ఎప్పుడు ఉపయోగించడం ఉత్తమమో ఆలోచించండి.

   గమనిక: మీరు ఎంచుకున్న coding agent ఈ features లో కొన్ని support చేయకపోవచ్చు. అలాంటప్పుడు వాటిని దాటవేయవచ్చు లేదా వాటికి మద్దతు ఇచ్చే వేరే coding agent ను ప్రయత్నించవచ్చు.

5. [Code Quality lecture](/2026/code-quality/) లోని Markdown bullet points regex exercise లో ఉన్న అదే లక్ష్యాన్ని సాధించడానికి coding agent ను ఉపయోగించండి.

   ఈ ప్రశ్నలకు సమాధానాలు కనుగొనండి:

   - Agent నేరుగా file ను edit చేసి task ను పూర్తి చేస్తుందా?
   - ఈ విధంగా file ను నేరుగా edit చేయడం వల్ల కలిగే లోపాలు (downsides) మరియు పరిమితులు (limitations) ఏమిటి?
   - Agent నేరుగా file edits చేయకుండా task ను పూర్తి చేసేలా ఎలా prompt ఇవ్వాలి?

   సూచన (Hint): [మొదటి lecture](/2026/course-shell/) లో పేర్కొన్న command-line tools లో ఒకదాన్ని ఉపయోగించమని agent ను అడగండి.

6. చాలా coding agents లో "yolo mode" అనే విధానం ఉంటుంది (ఉదాహరణకు, Claude Code లో `--dangerously-skip-permissions`).

   ఈ mode ను నేరుగా ఉపయోగించడం భద్రతాపరంగా (security-wise) సురక్షితం కాదు. అయితే, virtual machine లేదా container వంటి isolated environment లో coding agent ను నడిపి, తర్వాత autonomous operation ను enable చేయడం ఆమోదయోగ్యంగా ఉండవచ్చు.

   మీ machine పై ఈ setup ను అమలు చేసి చూడండి.

   సహాయపడే documentation:

   - [Claude Code Dev Containers](https://code.claude.com/docs/en/devcontainer)
   - [Docker Sandboxes / Claude Code](https://docs.docker.com/ai/sandboxes/agents/claude-code/)

   ఈ setup ను అమలు చేయడానికి ఒకటి కంటే ఎక్కువ మార్గాలు ఉన్నాయని గమనించండి.
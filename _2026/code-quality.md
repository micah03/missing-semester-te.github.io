---
layout: lecture
title: "Code Quality"
description: >
  Formatting, Linting, Testing, Continuous Integration మరియు ఇతర ముఖ్యమైన Code Quality పద్ధతుల గురించి తెలుసుకోండి.
thumbnail: /static/assets/thumbnails/2026/lec9.png
date: 2026-01-23
ready: true
video:
  aspect: 56.25
  id: XBiLUNx84CQ
---

అధిక నాణ్యత (high-quality) గల code రాయడంలో developers కు సహాయపడే అనేక tools మరియు techniques ఉన్నాయి.

ఈ lecture లో మనం ఈ క్రింది అంశాలను చర్చిస్తాము:

- [Formatting](#formatting)
- [Linting](#linting)
- [Testing](#testing)
- [Pre-commit Hooks](#pre-commit-hooks)
- [Continuous Integration](#continuous-integration)
- [Command Runners](#command-runners)

అదనపు అంశంగా (bonus topic), మనం [Regular Expressions](#regular-expressions) గురించి కూడా చర్చిస్తాము.

ఇది అనేక రంగాల్లో ఉపయోగపడే (cross-cutting) విషయం.

ఉదాహరణకు:

- Code Quality లో, ఒక నిర్దిష్ట pattern కు సరిపడే tests ను మాత్రమే run చేయడానికి
- IDEs లో, search and replace operations నిర్వహించడానికి

Regular Expressions ఉపయోగపడతాయి.

ఈ lecture లో చర్చించే tools లో చాలావరకు language-specific గా ఉంటాయి.

ఉదాహరణకు:

- Python కోసం [Ruff](https://docs.astral.sh/ruff/) linter/formatter

కొన్ని tools మాత్రం బహుళ programming languages ను support చేస్తాయి.

ఉదాహరణకు:

- [Prettier](https://prettier.io/) code formatter

అయితే, underlying concepts మాత్రం దాదాపు అన్ని programming languages లో ఒకే విధంగా ఉంటాయి.

ఏ programming language అయినా, సాధారణంగా మీరు ఈ తరహా tools ను కనుగొనవచ్చు:

- Code Formatters
- Linters
- Testing Libraries
- Static Analysis Tools
- మరియు ఇతర Code Quality tools

అందువల్ల, ఇక్కడ నేర్చుకునే సూత్రాలు (concepts) ఒకే language కు పరిమితం కావు; అవి దాదాపు అన్ని software development ecosystems లో వర్తిస్తాయి.

# Formatting

Code Auto-Formatters, code యొక్క surface syntax ను స్వయంచాలకంగా (automatically) శుభ్రంగా మరియు స్థిరంగా (consistent) మారుస్తాయి.

దీంతో మీరు లోతైన (deep) మరియు క్లిష్టమైన (challenging) సమస్యలపై దృష్టి పెట్టవచ్చు. అదే సమయంలో, formatting tool సాధారణమైన (mundane) విషయాలను చూసుకుంటుంది.

ఉదాహరణకు:

- Strings కోసం `'` లేదా `"` syntax ను ఒకే విధంగా ఉంచడం
- Binary operators చుట్టూ spaces ఉంచడం (`x+y` బదులుగా `x + y`)
- `import` statements ను క్రమబద్ధంగా (sorted order) అమర్చడం
- చాలా పొడవైన (over-length) lines ను నివారించడం

Code Formatters యొక్క ప్రధాన ప్రయోజనాలలో ఒకటి, ఒకే codebase పై పనిచేసే అన్ని developers మధ్య code style ను ప్రమాణీకరించడం (standardize చేయడం).

కొన్ని tools, ఉదాహరణకు Prettier, చాలా ఎక్కువగా configurable గా ఉంటాయి.

అంటే, formatting behaviour ను మీ అవసరాలకు అనుగుణంగా మార్చుకోవచ్చు.

అలాంటి సందర్భాల్లో, configuration file ను project యొక్క [Version Control](/2026/version-control/) లో commit చేయడం మంచిది.

ఇంకొన్ని tools, ఉదాహరణకు:

- [Black](https://github.com/psf/black)
- [gofmt](https://pkg.go.dev/cmd/gofmt)

చాలా తక్కువ configurability లేదా ఎలాంటి configurability ఇవ్వవు.

దీనికి ఒక ముఖ్యమైన కారణం ఉంది:

**Bikeshedding** ను తగ్గించడం.

అంటే, అసలు ముఖ్యమైన engineering సమస్యల కంటే, చిన్న stylistic విషయాలపై అనవసరమైన చర్చలు జరగకుండా చూడడం.

మీ Code Formatter ను IDE తో integrate చేయవచ్చు.

దీంతో:

- మీరు type చేస్తున్నప్పుడు
- లేదా file ను save చేస్తున్నప్పుడు

code స్వయంచాలకంగా format అవుతుంది.

అదనంగా, project లో ఒక [EditorConfig](https://editorconfig.org/) file ను కూడా చేర్చవచ్చు.

ఈ file ద్వారా IDE కు project-level settings గురించి సమాచారం అందించవచ్చు.

ఉదాహరణకు:

- Indentation size
- Tab లేదా spaces వినియోగం
- వివిధ file types కు సంబంధించిన formatting preferences

ఇలాంటి settings ను project అంతటా ఒకే విధంగా అమలు చేయవచ్చు.

# Linting

Linters, **Static Analysis** ను ఉపయోగించి (అంటే code ను run చేయకుండా విశ్లేషించి) మీ code లోని antipatterns మరియు సంభావ్య (potential) సమస్యలను గుర్తిస్తాయి.

ఈ tools, auto-formatters కంటే మరింత లోతుగా (deeper) విశ్లేషిస్తాయి. అవి కేవలం surface syntax ను మాత్రమే కాకుండా, code యొక్క నిర్మాణం (structure) మరియు నాణ్యత (quality) ను కూడా పరిశీలిస్తాయి.

విశ్లేషణ యొక్క లోతు (depth of analysis) tool ను బట్టి మారుతూ ఉంటుంది.

Linters లో సాధారణంగా అనేక **Rules** ఉంటాయి.

ఈ rules కోసం project స్థాయిలో (project-level) configure చేయగల presets కూడా అందుబాటులో ఉంటాయి.

కొన్ని linter rules, కొన్నిసార్లు false positives ను ఉత్పత్తి చేయవచ్చు.

అలాంటి సందర్భాల్లో, మీరు ఆ rules ను:

- ఒక నిర్దిష్ట file కోసం
- లేదా ఒక నిర్దిష్ట line కోసం

disable చేయవచ్చు.

మంచి linters, ప్రతి rule గురించి స్పష్టమైన documentation లేదా built-in help ను అందిస్తాయి.

అందులో సాధారణంగా ఈ విషయాలు ఉంటాయి:

- ఆ rule ఏమి గుర్తించడానికి ప్రయత్నిస్తోంది?
- ఆ pattern ఎందుకు సమస్యాత్మకమైనది?
- దానికి బదులుగా ఏ విధానం మెరుగైనది?

ఉదాహరణకు, [Ruff](https://docs.astral.sh/ruff/) లోని [SIM102](https://docs.astral.sh/ruff/rules/collapsible-if/) rule, Python code లో అనవసరంగా nested అయిన `if` statements ను గుర్తిస్తుంది.

కొన్ని linters కేవలం సమస్యలను గుర్తించడం మాత్రమే కాకుండా, వాటిలో కొన్నింటిని స్వయంచాలకంగా (automatically) సరిచేయగలవు కూడా.

Language-specific linters తో పాటు, మరో ఉపయోగకరమైన tool **semgrep**.

[semgrep](https://github.com/semgrep/semgrep) అనేది ఒక **Semantic Grep** tool.

సాధారణ `grep` character స్థాయిలో (character level) పనిచేస్తుంది.

కానీ semgrep, **AST (Abstract Syntax Tree)** స్థాయిలో పనిచేస్తుంది.

దీంతో code యొక్క అర్థాన్ని (semantics) బట్టి patterns ను గుర్తించగలదు.

ఇది అనేక programming languages ను support చేస్తుంది.

మీ projects కోసం custom linter rules రూపొందించడానికి semgrep చాలా ఉపయోగపడుతుంది.

ఉదాహరణకు, Python లో ప్రమాదకరమైన:

```python
subprocess.Popen(..., shell=True)
```

pattern ను ఉపయోగించకుండా నిరోధించాలనుకుంటే, semgrep ద్వారా ఆ code ను సులభంగా గుర్తించవచ్చు:

```bash
semgrep -l python -e "subprocess.Popen(..., shell=True, ...)"
```

ఈ command, Python files లో `subprocess.Popen(..., shell=True)` ఉపయోగించిన అన్ని స్థానాలను గుర్తిస్తుంది.

# Testing

Software Testing అనేది మీ code సరైనదిగా (correct) పనిచేస్తుందనే నమ్మకాన్ని పెంచడానికి ఉపయోగించే ప్రామాణిక (standard) పద్ధతి.

దీని ప్రాథమిక ఆలోచన:

- ముందుగా మీరు code రాస్తారు
- తర్వాత, ఆ code ను పరీక్షించే (exercise చేసే) మరో code ను రాస్తారు
- ఆశించిన విధంగా పనిచేయకపోతే, test error ను చూపిస్తుంది

Tests ను వివిధ స్థాయిలలో (levels of granularity) రాయవచ్చు:

- **Unit Tests** — ఒక్కో function లేదా చిన్న code unit ను పరీక్షిస్తాయి
- **Integration Tests** — modules లేదా services మధ్య పరస్పర చర్యలను (interactions) పరీక్షిస్తాయి
- **Functional Tests** — పూర్తిస్థాయి end-to-end scenarios ను పరీక్షిస్తాయి

మీరు **Test-Driven Development (TDD)** ను కూడా అనుసరించవచ్చు.

ఈ విధానంలో:

1. ముందుగా tests రాస్తారు
2. తర్వాత ఆ tests ను pass చేయడానికి implementation code రాస్తారు

మీ code లో bugs కనిపించినప్పుడు, వాటి కోసం **Regression Tests** రాయవచ్చు.

దీంతో:

- ఆ bug ఒకసారి fix అయిన తర్వాత
- భవిష్యత్తులో అదే functionality మళ్లీ చెడిపోతే

test వెంటనే గుర్తిస్తుంది.

మీరు **Property-Based Tests** కూడా రాయవచ్చు.

ఈ విధానాన్ని Haskell లోని [QuickCheck](https://hackage.haskell.org/package/QuickCheck) ప్రాచుర్యంలోకి తీసుకువచ్చింది.

ఇప్పుడు అనేక languages లో దీనికి implementations ఉన్నాయి.

ఉదాహరణకు:

- Python కోసం [Hypothesis](https://hypothesis.readthedocs.io/)

Property-Based Testing లో, ప్రతి input-output pair కోసం tests రాయడం కంటే, code ఎప్పుడూ పాటించాల్సిన properties లేదా rules ను నిర్వచిస్తారు.

ఏ testing approach ఉపయోగించాలో మీ project అవసరాలపై ఆధారపడి ఉంటుంది.

చాలా సందర్భాల్లో, ఒకే విధానాన్ని కాకుండా అనేక testing techniques కలయికను (combination) ఉపయోగిస్తారు.

మీ application కు బాహ్య ఆధారాలు (external dependencies) ఉంటే, ఉదాహరణకు:

- Database
- Web API
- Third-party Service

Tests సమయంలో వాటితో నేరుగా communicate చేయడం ఎల్లప్పుడూ ఉత్తమం కాదు.

అలాంటి సందర్భాల్లో, ఆ dependencies ను **Mock** చేయడం ఉపయోగకరంగా ఉంటుంది.

Mocking వల్ల:

- Tests వేగంగా run అవుతాయి
- External services అందుబాటులో లేకపోయినా tests పనిచేస్తాయి
- Third-party systems వల్ల tests విఫలమయ్యే అవకాశాలు తగ్గుతాయి
- Tests మరింత స్థిరంగా (reliable) మారుతాయి

అందువల్ల, external dependencies ఉన్న projects లో mocking ఒక ముఖ్యమైన testing technique గా పరిగణించబడుతుంది.

## Code Coverage

**Code Coverage** అనేది మీ tests ఎంత సమర్థవంతంగా ఉన్నాయో కొలవడానికి ఉపయోగించే ఒక metric.

Tests run చేసినప్పుడు, మీ code లోని ఏ lines execute అయ్యాయో Code Coverage పరిశీలిస్తుంది.

దీంతో:

- అన్ని code paths పరీక్షించబడుతున్నాయో లేదో తెలుసుకోవచ్చు
- ఇంకా test చేయని భాగాలను గుర్తించవచ్చు
- కొత్త tests ఎక్కడ అవసరమో అర్థం చేసుకోవచ్చు

చాలా Code Coverage tools, line-by-line coverage వివరాలను చూపిస్తాయి.

దీని వల్ల, coverage తక్కువగా ఉన్న ప్రాంతాలను గుర్తించి వాటి కోసం tests రాయడం సులభమవుతుంది.

[Codecov](https://app.codecov.io) వంటి services, project చరిత్ర అంతటా Code Coverage ను track చేయడానికి మరియు visual గా చూడడానికి web interfaces ను అందిస్తాయి.

అయితే, ప్రతి metric లాగే Code Coverage కూడా పరిపూర్ణమైనది కాదు.

Coverage శాతంపై మాత్రమే దృష్టి పెట్టకండి.

ఉదాహరణకు:

- 100% coverage ఉన్న tests కూడా నాణ్యత లేనివి కావచ్చు
- 80% coverage ఉన్న tests చాలా బలమైనవిగా ఉండవచ్చు

అందువల్ల, coverage సంఖ్య కంటే **అధిక నాణ్యత గల tests** రాయడంపైనే ప్రధాన దృష్టి ఉండాలి.

# Pre-commit Hooks

Git లోని **Pre-commit Hooks** అనేవి, ప్రతి commit కు ముందు స్వయంచాలకంగా (automatically) run అయ్యే scripts.

Git Hooks ను ఉపయోగించడం సులభతరం చేయడానికి [pre-commit](https://pre-commit.com/) framework విస్తృతంగా ఉపయోగించబడుతుంది.

Pre-commit hooks యొక్క ముఖ్య ఉద్దేశ్యం:

Commit చేయబడే code, project standards కు అనుగుణంగా ఉందో లేదో ముందుగానే నిర్ధారించడం.

చాలా projects లో, Pre-commit hooks ద్వారా ఈ పనులు స్వయంచాలకంగా నిర్వహించబడతాయి:

- Formatters ను run చేయడం
- Linters ను run చేయడం
- కొన్నిసార్లు Tests ను కూడా run చేయడం

ఇవి ప్రతి commit కు ముందు execute అవుతాయి.

దీంతో:

- Commit చేసిన code project యొక్క code style ను అనుసరిస్తుంది
- సాధారణ తప్పులు (common issues) ముందుగానే గుర్తించబడతాయి
- Review సమయంలో అనవసరమైన formatting లేదా linting సమస్యలు తగ్గుతాయి
- Code Quality మరింత స్థిరంగా ఉంటుంది

అంటే, Pre-commit Hooks అనేవి "చెడు code repository లోకి వెళ్లకముందే" దాన్ని తనిఖీ చేసే మొదటి రక్షణ పొర (first line of defense) లాంటివి.

# Continuous Integration

**Continuous Integration (CI)** services, ఉదాహరణకు [GitHub Actions](https://github.com/features/actions), మీరు code ను push చేసిన ప్రతిసారి (లేదా ప్రతి Pull Request పై, లేదా నిర్దిష్ట schedule ప్రకారం) scripts ను స్వయంచాలకంగా run చేయగలవు.

Developers సాధారణంగా CI services ను వివిధ Code Quality tools ను అమలు చేయడానికి ఉపయోగిస్తారు.

ఉదాహరణకు:

- Formatters
- Linters
- Tests

Compiled languages లో, CI ద్వారా code విజయవంతంగా compile అవుతుందో లేదో నిర్ధారించవచ్చు.

Statically Typed Languages లో, code type checking ను విజయవంతంగా pass అవుతుందో లేదో ధృవీకరించవచ్చు.

కొత్త commits push చేసిన ప్రతిసారి CI ను run చేయడం వల్ల, code యొక్క ప్రధాన version (main branch) లోకి చేరిన errors ను త్వరగా గుర్తించవచ్చు.

Pull Requests పై CI ను run చేయడం వల్ల:

- Contributors పంపిన changes లోని సమస్యలను merge కు ముందే గుర్తించవచ్చు.

Schedule ప్రకారం CI jobs ను run చేయడం వల్ల:

- External dependencies కారణంగా వచ్చే సమస్యలను గుర్తించవచ్చు.

ఉదాహరణకు:

ఒక developer పొరపాటున breaking change ను విడుదల చేసినా, దాన్ని [SemVer-compatible](/2026/shipping-code/#releases--versioning) release గా ప్రకటించి ఉండవచ్చు.

అలాంటి పరిస్థితుల్లో scheduled CI runs సమస్యను ముందుగానే గుర్తించగలవు.

CI scripts, developers machines నుండి పూర్తిగా వేరుగా (independently) run అవుతాయి.

దీని వల్ల, developer machine పై run చేయడానికి ఎక్కువ సమయం పట్టే jobs ను కూడా CI లో సులభంగా అమలు చేయవచ్చు.

దీనికి ఒక మంచి ఉదాహరణ **Test Matrix**.

Test Matrix ద్వారా:

- వేర్వేరు Operating Systems పై
- వేర్వేరు Programming Language versions పై

ఒకే test suite ను run చేయవచ్చు.

దీంతో software అన్ని environments లో సరిగ్గా పనిచేస్తుందో లేదో నిర్ధారించవచ్చు.

సాధారణంగా, CI లో run అయ్యే scripts మీ code ను నేరుగా మార్చవు.

అవి ఎక్కువగా tools ను **check-only mode** లో run చేస్తాయి.

**fix mode** లో కాదు.

ఉదాహరణకు:

- Auto-formatter, code ను స్వయంగా format చేయదు
- బదులుగా, code formatting rules కు అనుగుణంగా లేకపోతే error ను చూపిస్తుంది

దీంతో developers:

- సమస్యలను గుర్తించగలరు
- అవసరమైన మార్పులను స్థానికంగా (locally) చేయగలరు
- తర్వాత సరిచేసిన code ను మళ్లీ push చేయగలరు

అంటే, CI యొక్క ప్రధాన లక్ష్యం code ను ఆటోమేటిక్‌గా సరిచేయడం కాదు; సమస్యలను గుర్తించి వాటిని developers దృష్టికి తీసుకురావడం.

Repositories లో తరచుగా README లో **Status Badges** ను చేర్చుతారు.

ఈ badges సాధారణంగా ఈ సమాచారాన్ని చూపిస్తాయి:

- CI (Continuous Integration) Build Status
- Code Coverage
- Test Results
- ఇతర Project Health Metrics

ఉదాహరణకు, Missing Semester project యొక్క ప్రస్తుత build status ను README లో badges ద్వారా చూపిస్తారు.

```md
[![Build Status](https://github.com/missing-semester/missing-semester/actions/workflows/build.yml/badge.svg)](...)
[![Links Status](https://github.com/missing-semester/missing-semester/actions/workflows/links.yml/badge.svg)](...)
```

ఈ badges ద్వారా, project ప్రస్తుతం సక్రమంగా build అవుతోందా లేదా tests విజయవంతంగా run అవుతున్నాయా అనే విషయాన్ని ఒక చూపులోనే తెలుసుకోవచ్చు.

> మా Links Checker తరచుగా fail అవుతూ ఉంటుంది. సాధారణంగా దీనికి కారణం third-party websites లోని సమస్యలే.
>
> అయినప్పటికీ, ఈ checker మాకు అనేక broken links ను గుర్తించి సరిచేయడంలో సహాయపడింది.
>
> కొన్ని సందర్భాల్లో అవి typing mistakes వల్ల వస్తాయి.
>
> కానీ ఎక్కువగా:
>
> - Websites తమ content ను వేరే చోటుకు మార్చడం
> - Redirects ఇవ్వకుండా URLs మార్చడం
> - లేదా Websites పూర్తిగా కనుమరుగవడం
>
> వంటి కారణాల వల్ల broken links ఏర్పడతాయి.

CI Services, Formatters, Linters మరియు Testing Libraries గురించి లోతుగా నేర్చుకోవడానికి ఉత్తమ మార్గాలలో ఒకటి **ఉత్తమ Open Source Projects ను అధ్యయనం చేయడం**.

GitHub లో ఉన్న అధిక నాణ్యత గల projects ను పరిశీలించండి.

సాధ్యమైనంత వరకు, మీ project కు ఈ అంశాల్లో దగ్గరగా ఉండే projects ను ఎంచుకోండి:

- Programming Language
- Domain
- Project Size
- Project Scope
- Architecture

ఇలాంటి projects ను అధ్యయనం చేయడం వల్ల, వాస్తవ ప్రపంచంలో (real-world) best practices ఎలా అమలు చేయబడుతున్నాయో తెలుసుకోవచ్చు.

ప్రత్యేకంగా ఈ files ను పరిశీలించండి:

- `pyproject.toml`
- `.github/workflows/`
- `DEVELOPMENT.md`
- `CONTRIBUTING.md`
- `README.md`
- మరియు ఇతర development-related configuration files

ఈ files ద్వారా మీరు:

- Formatting ఎలా configure చేశారో
- Linting ఎలా అమలు చేస్తున్నారో
- Tests ఎలా run చేస్తున్నారో
- CI Pipelines ఎలా రూపొందించారో
- Development workflows ఎలా నిర్వహిస్తున్నారో

నేరుగా నేర్చుకోవచ్చు.

చాలా సందర్భాల్లో, documentation చదవడం కంటే మంచి Open Source Project ను పరిశీలించడం ద్వారా మరింత వేగంగా మరియు ప్రాయోగికంగా (practically) నేర్చుకోవచ్చు.

## Continuous Deployment

**Continuous Deployment (CD)** అనేది Continuous Integration (CI) infrastructure ను ఉపయోగించి, changes ను స్వయంచాలకంగా (automatically) deploy చేసే ప్రక్రియ.

ఉదాహరణకు, Missing Semester repository లో Continuous Deployment ను GitHub Pages తో ఉపయోగిస్తున్నారు.

దీని వల్ల:

- Lecture Notes లో మార్పులు చేసి
- `git push` చేసిన వెంటనే

website స్వయంచాలకంగా:

1. Build అవుతుంది
2. Deploy అవుతుంది

అంటే, manual deployment steps అవసరం ఉండవు.

CI ద్వారా కేవలం websites మాత్రమే కాదు, ఇతర రకాల **Artifacts** ను కూడా build చేయవచ్చు.

ఉదాహరణలు:

- Application Binaries
- Docker Images
- Release Packages
- Deployment Bundles

అంటే, CI/CD pipeline software ను build చేయడం నుంచి production లో deploy చేయడం వరకు మొత్తం ప్రక్రియను ఆటోమేట్ చేయగలదు.

# Command Runners

**Command Runners** అనేవి project సందర్భంలో commands ను సులభంగా run చేయడానికి ఉపయోగించే tools.

ఒక project పెరుగుతున్న కొద్దీ, developers అనేక Code Quality tools ను run చేయాల్సి వస్తుంది.

ఉదాహరణకు:

```bash
uv run ruff check --fix
```

లాంటి commands ను ప్రతి developer గుర్తుంచుకోవడం అనవసరమైన భారంగా మారుతుంది.

ఇక్కడ Command Runners ఉపయోగపడతాయి.

ఉదాహరణకు, [just](https://github.com/casey/just) ను ఉపయోగిస్తే:

```bash
just lint
```

అనే సులభమైన command వెనుక:

```bash
uv run ruff check --fix
```

లేదా మరింత క్లిష్టమైన command run కావచ్చు.

అదే విధంగా:

```bash
just format
```

```bash
just typecheck
```

```bash
just test
```

వంటి commands ను కూడా నిర్వచించవచ్చు.

దీంతో developers:

- అసలు tool commands గుర్తుంచుకోవాల్సిన అవసరం ఉండదు
- Project workflows మరింత సరళంగా ఉంటాయి
- కొత్త developers onboarding సులభమవుతుంది

కొన్ని language-specific package managers మరియు project managers లో ఈ functionality ఇప్పటికే built-in గా ఉంటుంది.

అలాంటి సందర్భాల్లో, `just` వంటి language-agnostic tool ను ఉపయోగించాల్సిన అవసరం ఉండకపోవచ్చు.

ఉదాహరణలు:

### Node.js (npm)

`package.json` లోని:

```json
"scripts": {
  "lint": "...",
  "test": "...",
  "build": "..."
}
```

section ద్వారా commands ను నిర్వచించవచ్చు.

తర్వాత:

```bash
npm run lint
```

```bash
npm run test
```

వంటి commands ను ఉపయోగించవచ్చు.

### Python (Hatch)

`pyproject.toml` లోని:

```toml
tool.hatch.envs.*.scripts
```

sections ద్వారా scripts ను నిర్వచించవచ్చు.

దీంతో developers ఒకే విధమైన, సులభమైన commands ద్వారా project tooling ను ఉపయోగించగలరు.

మొత్తానికి, Command Runners యొక్క ప్రధాన లక్ష్యం:

- Commands ను గుర్తుంచుకోవాల్సిన అవసరాన్ని తగ్గించడం
- Development workflows ను ప్రమాణీకరించడం (standardize)
- Project tooling ను ఉపయోగించడం సులభతరం చేయడం

# Regular Expressions

**Regular Expressions**, సాధారణంగా **Regex** అని పిలుస్తారు, అనేది strings సమితులను (sets of strings) వివరించడానికి ఉపయోగించే ఒక ప్రత్యేక భాష (language).

Regex patterns ను వివిధ సందర్భాల్లో pattern matching కోసం విస్తృతంగా ఉపయోగిస్తారు.

ఉదాహరణకు:

- Command-line tools
- IDEs
- Search tools
- Programming languages

వంటి అనేక చోట్ల Regex ఉపయోగపడుతుంది.

ఉదాహరణకు, [ag](https://github.com/ggreer/the_silver_searcher) అనే tool, మొత్తం codebase అంతటా regex ఆధారిత search ను support చేస్తుంది.

ఉదాహరణ:

```bash
ag "import .* as .*"
```

ఈ command, Python లో rename చేసిన imports అన్నింటినీ కనుగొంటుంది.

అదేవిధంగా, [go test](https://pkg.go.dev/cmd/go#hdr-Test_packages) command లో:

```bash
-run [regexp]
```

అనే option ఉంటుంది.

దీని ద్వారా, regex pattern కు సరిపడే tests మాత్రమే run చేయవచ్చు.

అలాగే, చాలా programming languages లో:

- Built-in Regex support
- లేదా Third-party Regex libraries

ఉంటాయి.

వీటిని ఉపయోగించి:

- Pattern Matching
- Validation
- Parsing

వంటి పనులను చేయవచ్చు.

Regex గురించి అవగాహన పెంచుకోవడానికి, కొన్ని ఉదాహరణలు చూద్దాం.

ఈ lecture లో మనం **Python Regex Syntax** ను ఉపయోగిస్తున్నాము.

Regex కు అనేక flavors (variants) ఉన్నాయి.

వాటి మధ్య ముఖ్యంగా advanced features లో చిన్నచిన్న తేడాలు ఉండవచ్చు.

Regex ను రూపొందించడానికి మరియు debug చేయడానికి:

- [regex101](https://regex101.com/)

వంటి online tools చాలా ఉపయోగపడతాయి.

### Regex Examples

- `abc`

  `"abc"` అనే literal string ను match చేస్తుంది.

- `missing|semester`

  `"missing"` లేదా `"semester"` అనే strings లో ఏదో ఒకదాన్ని match చేస్తుంది.

- `\d{4}-\d{2}-\d{2}`

  YYYY-MM-DD format లో ఉన్న తేదీలను match చేస్తుంది.

  ఉదాహరణ:

  ```text
  2026-01-14
  ```

  అయితే, ఇది కేవలం format ను మాత్రమే తనిఖీ చేస్తుంది.

  అంటే:

  - నాలుగు అంకెలు
  - ఒక dash (`-`)
  - రెండు అంకెలు
  - మరో dash (`-`)
  - రెండు అంకెలు

  ఉన్నాయో లేదో మాత్రమే చూస్తుంది.

  తేదీ నిజంగా చెల్లుబాటు అవుతుందో లేదో (valid date కాదో) తనిఖీ చేయదు.

  కాబట్టి:

  ```text
  2026-01-99
  ```

  కూడా ఈ regex కు match అవుతుంది.

- `.+@.+`

  Email address లాంటి strings ను match చేస్తుంది.

  అంటే:

  - కొంత text
  - తర్వాత `@`
  - తర్వాత మరికొంత text

  ఉండాలి.

  ఉదాహరణ:

  ```text
  user@example.com
  ```

  అయితే, ఇది చాలా ప్రాథమిక (basic) validation మాత్రమే చేస్తుంది.

  అందువల్ల:

  ```text
  nonsense@@@email
  ```

  వంటి చెల్లని strings కూడా match కావచ్చు.

  False Positives మరియు False Negatives లేకుండా email addresses ను పూర్తిగా match చేసే regex ఒకటి ఉన్నప్పటికీ, అది చాలా క్లిష్టమైనది మరియు ప్రాయోగికంగా (practically) ఉపయోగించడం కష్టమైనది.

## Regex Syntax

Regex Syntax గురించి పూర్తి వివరాలు తెలుసుకోవాలంటే, [ఈ Documentation](https://docs.python.org/3/library/re.html#regular-expression-syntax) ను (లేదా ఆన్‌లైన్‌లో అందుబాటులో ఉన్న ఇతర Regex Guides ను) చూడవచ్చు.

కింద కొన్ని ప్రాథమిక Regex Building Blocks ఇవ్వబడ్డాయి:

- `abc`

  Characters కు ప్రత్యేక అర్థం (special meaning) లేనప్పుడు, literal string ను match చేస్తుంది.

  ఉదాహరణ:

  ```text
  abc
  ```

- `.`

  ఏదైనా ఒకే ఒక్క character ను match చేస్తుంది.

- `[abc]`

  Brackets లో ఉన్న characters లో ఏదో ఒకదాన్ని match చేస్తుంది.

  ఉదాహరణ:

  ```text
  a
  b
  c
  ```

- `[^abc]`

  Brackets లో ఉన్న characters మినహా మిగిలిన ఏదైనా ఒక్క character ను match చేస్తుంది.

  ఉదాహరణ:

  ```text
  d
  x
  5
  ```

- `[a-f]`

  సూచించిన range లోని ఒక్క character ను match చేస్తుంది.

  ఉదాహరణ:

  ```text
  c
  ```

  match అవుతుంది.

  ```text
  q
  ```

  match కాదు.

- `a|b`

  రెండు patterns లో ఏదో ఒకదాన్ని match చేస్తుంది.

  ఉదాహరణ:

  ```text
  a
  ```

  లేదా

  ```text
  b
  ```

- `\d`

  ఏదైనా digit (0-9) ను match చేస్తుంది.

  ఉదాహరణ:

  ```text
  3
  ```

- `\w`

  ఏదైనా word character ను match చేస్తుంది.

  సాధారణంగా:

  - అక్షరాలు (letters)
  - సంఖ్యలు (digits)
  - underscore (`_`)

  ఇందులో ఉంటాయి.

  ఉదాహరణ:

  ```text
  x
  ```

- `\b`

  Word Boundary ను match చేస్తుంది.

  ఉదాహరణకు:

  ```text
  missing semester
  ```

  అనే string లో:

  - `missing` ప్రారంభానికి ముందు
  - `missing` ముగిసిన వెంటనే
  - `semester` ప్రారంభానికి ముందు
  - `semester` ముగిసిన వెంటనే

  boundary locations వద్ద match అవుతుంది.

- `(...)`

  ఒక pattern ను group చేయడానికి ఉపయోగిస్తారు.

- `...?`

  Pattern యొక్క **సున్నా లేదా ఒక occurrence** ను match చేస్తుంది.

  ఉదాహరణ:

  ```regex
  words?
  ```

  ఇది:

  ```text
  word
  ```

  మరియు

  ```text
  words
  ```

  రెండింటినీ match చేస్తుంది.

- `...*`

  Pattern యొక్క **సున్నా లేదా అంతకంటే ఎక్కువ occurrences** ను match చేస్తుంది.

  ఉదాహరణ:

  ```regex
  .*
  ```

  ఏ సంఖ్యలోనైనా characters ను match చేయగలదు.

- `...+`

  Pattern యొక్క **ఒకటి లేదా అంతకంటే ఎక్కువ occurrences** ను match చేస్తుంది.

  ఉదాహరణ:

  ```regex
  \d+
  ```

  ఒకటి లేదా అంతకంటే ఎక్కువ digits ను match చేస్తుంది.

- `...{N}`

  Pattern ఖచ్చితంగా **N సార్లు** రావాలని సూచిస్తుంది.

  ఉదాహరణ:

  ```regex
  \d{4}
  ```

  ఖచ్చితంగా నాలుగు digits ను match చేస్తుంది.

- `\.`

  Literal dot (`.`) character ను match చేస్తుంది.

  ఎందుకంటే regex లో `.` కి ప్రత్యేక అర్థం ఉంది.

- `\\`

  Literal backslash (`\`) ను match చేస్తుంది.

- `^`

  Line ప్రారంభాన్ని (start of line) match చేస్తుంది.

- `$`

  Line ముగింపును (end of line) match చేస్తుంది.

## Capture Groups మరియు References

Regex లో groups `(...)` ఉపయోగించినప్పుడు, match అయిన string లోని నిర్దిష్ట భాగాలను (sub-parts) విడిగా సూచించవచ్చు.

ఇవి ప్రధానంగా ఈ పనుల కోసం ఉపయోగపడతాయి:

- Data Extraction
- Search and Replace
- Pattern Reuse

ఉదాహరణకు, YYYY-MM-DD format లో ఉన్న తేదీ నుండి నెల (month) భాగాన్ని మాత్రమే తీసుకోవాలనుకుందాం.

ఈ Python code ను చూడండి:

```python
>>> import re
>>> re.match(r"\d{4}-(\d{2})-\d{2}", "2026-01-14").group(1)
'01'
```

ఇక్కడ:

```regex
(\d{2})
```

అనేది ఒక Capture Group.

Regex match అయిన తర్వాత:

```python
.group(1)
```

మొదటి capture group లోని విలువను తిరిగి ఇస్తుంది.

అందువల్ల:

```text
01
```

ఫలితంగా వస్తుంది.

Text Editors మరియు IDEs లో కూడా Capture Groups చాలా ఉపయోగపడతాయి.

ప్రత్యేకంగా Search and Replace సమయంలో.

Replace Pattern లో, match అయిన groups ను మళ్లీ ఉపయోగించవచ్చు.

Syntax IDE ను బట్టి మారుతుంది.

ఉదాహరణకు:

### VS Code

```text
$1
$2
$3
```

మొదటి, రెండో, మూడో capture groups ను సూచిస్తాయి.

### Vim

```text
\1
\2
\3
```

అదే పనిని చేస్తాయి.

దీంతో, క్లిష్టమైన text transformations ను చాలా సులభంగా చేయవచ్చు.

## పరిమితులు (Limitations)

Regex ఆధారపడే **Regular Languages** చాలా శక్తివంతమైనవే అయినప్పటికీ, వాటికి కొన్ని పరిమితులు ఉన్నాయి.

కొన్ని string patterns ను సాధారణ Regex ద్వారా వ్యక్తీకరించడం సాధ్యం కాదు.

ఉదాహరణకు:

```text
aⁿbⁿ
```

అంటే:

```text
ab
aabb
aaabbb
aaaabbbb
...
```

వంటి strings.

ఇక్కడ:

- "a" ల సంఖ్య
- "b" ల సంఖ్య

ఖచ్చితంగా సమానంగా ఉండాలి.

ఇలాంటి pattern ను సాధారణ Regular Expression ద్వారా వ్రాయడం సాధ్యం కాదు.

ఇది Regular Languages యొక్క సిద్ధాంతపరమైన (theoretical) పరిమితుల్లో ఒకటి.

మరింత ప్రాయోగిక ఉదాహరణ:

**HTML** ఒక Regular Language కాదు.

అందువల్ల:

> "Regex తో HTML ను parse చేద్దాం"

అనే విధానం సాధారణంగా సరైనది కాదు.

ఆధునిక Regex Engines కొన్ని advanced features ను అందిస్తాయి.

ఉదాహరణకు:

- Lookahead
- Lookbehind
- Backreferences

ఇలాంటి features వల్ల, Regex సామర్థ్యం సాధారణ Regular Languages కంటే ఎక్కువ అవుతుంది.

అందువల్ల, అవి వాస్తవ ప్రపంచంలో (real-world) చాలా ఉపయోగకరంగా ఉంటాయి.

అయితే, వాటి వ్యక్తీకరణ సామర్థ్యానికి (expressive power) ఇంకా పరిమితులు ఉన్నాయని గుర్తుంచుకోవాలి.

భాష (language) లేదా data structure మరింత క్లిష్టంగా ఉంటే, Regex సరిపోకపోవచ్చు.

అలాంటి సందర్భాల్లో, మరింత శక్తివంతమైన **Parser** అవసరం అవుతుంది.

ఉదాహరణకు:

- [pyparsing](https://github.com/pyparsing/pyparsing)

ఇది ఒక Parsing Library.

ఇది **PEG (Parsing Expression Grammar)** ఆధారంగా పనిచేస్తుంది.

Regex తో కష్టమైన లేదా అసాధ్యమైన parsing పనులను ఇలాంటి parsers సులభంగా నిర్వహించగలవు.

## Regex నేర్చుకోవడం (Learning Regex)

Regex లోని ప్రాథమిక అంశాలను (ఈ lecture లో చర్చించిన విషయాలను) బాగా అర్థం చేసుకోవాలని మేము సిఫారసు చేస్తున్నాము.

అయితే, మొత్తం Regex language ను కంఠస్థం (memorize) చేయడానికి ప్రయత్నించాల్సిన అవసరం లేదు.

దానికి బదులుగా:

- Fundamentals ను నేర్చుకోండి
- అవసరం వచ్చినప్పుడు Regex References లేదా Documentation ను చూడండి

అనే విధానం మరింత ప్రయోజనకరంగా ఉంటుంది.

ప్రస్తుతం Conversational AI tools కూడా Regex patterns రూపొందించడంలో చాలా ఉపయోగకరంగా ఉన్నాయి.

ఉదాహరణకు, మీకు ఇష్టమైన LLM ను ఈ విధంగా అడగవచ్చు:

```text
Write a Python-style regex pattern that matches the requested path from log lines from Nginx. Here is an example log line:

169.254.1.1 - - [09/Jan/2026:21:28:51 +0000] "GET /feed.xml HTTP/2.0" 200 2995 "-" "python-requests/2.32.3"
```

ఇలాంటి prompt ద్వారా, LLM:

- Log format ను విశ్లేషించగలదు
- అవసరమైన భాగాన్ని గుర్తించగలదు
- సరైన Regex pattern ను సూచించగలదు

అయితే, AI రూపొందించిన Regex ను నేరుగా నమ్మకుండా:

- అది ఎలా పనిచేస్తుందో అర్థం చేసుకోవడం
- వివిధ inputs పై పరీక్షించడం
- Edge cases ను పరిశీలించడం

మంచి అలవాటు.

Regex విషయంలో AI ను "సహాయక సాధనం" (assistant) గా ఉపయోగించడం ఉత్తమం; "అంతిమ సత్యం" (source of truth) గా కాదు.

# అభ్యాసాలు (Exercises)

1. మీరు ప్రస్తుతం పని చేస్తున్న project కోసం ఒక Formatter, Linter మరియు Pre-commit Hooks ను configure చేయండి.

   Errors ఎక్కువగా ఉంటే:

   - Formatting errors ను Autoformatter ద్వారా సరిచేయండి.
   - Linter errors ను సరిచేయడానికి ఒక [AI Agent](/2026/agentic-coding/) ను ఉపయోగించి చూడండి.

   ముఖ్యంగా:

   - AI Agent కు linter ను run చేసే అవకాశం ఇవ్వండి.
   - Linter results ను చదవగలిగేలా చేయండి.
   - తద్వారా అది iterative loop లో errors ను సరిచేయగలదు.

   చివరగా, ఫలితాలను జాగ్రత్తగా పరిశీలించండి.

   AI code ను సరిచేస్తూ కొత్త bugs ను సృష్టించలేదని నిర్ధారించుకోండి.

2. మీకు తెలిసిన programming language లోని ఒక testing library ను నేర్చుకోండి.

   మీరు పని చేస్తున్న project కోసం ఒక Unit Test రాయండి.

   తర్వాత:

   - Code Coverage Tool ను run చేయండి
   - HTML Coverage Report ను generate చేయండి
   - ఫలితాలను పరిశీలించండి

   ఆలోచించండి:

   - ఏ lines cover అయ్యాయి?
   - Coverage ఎంత ఉంది?

   సాధారణంగా మొదట్లో coverage చాలా తక్కువగా ఉంటుంది.

   Coverage పెంచడానికి:

   - కొన్ని tests ను స్వయంగా రాయండి
   - తర్వాత [AI Agent](/2026/agentic-coding/) సహాయంతో coverage ను మెరుగుపరచడానికి ప్రయత్నించండి

   Agent కు:

   - Coverage తో tests run చేసే అవకాశం
   - Line-by-line coverage report చూసే అవకాశం

   ఇవ్వండి.

   తర్వాత పరిశీలించండి:

   - AI రూపొందించిన tests నిజంగా మంచి tests అవునా?
   - లేక కేవలం coverage percentage పెంచడానికి మాత్రమే రాయబడినవా?

3. మీరు పని చేస్తున్న project కోసం Continuous Integration (CI) ను configure చేయండి.

   ప్రతి `git push` పై CI run అయ్యేలా ఏర్పాటు చేయండి.

   CI ద్వారా:

   - Formatting Checks
   - Linting
   - Tests

   run చేయండి.

   తర్వాత ఉద్దేశపూర్వకంగా (intentionally) code ను చెడగొట్టండి.

   ఉదాహరణకు:

   - ఒక Linter violation ను చేర్చండి

   CI ఆ సమస్యను గుర్తిస్తుందో లేదో నిర్ధారించండి.

4. ఒక [Regex Pattern](#regular-expressions) రాసి, `grep` [Command-Line Tool](/2026/course-shell/) ను ఉపయోగించి మీ code లో:

   ```python
   subprocess.Popen(..., shell=True)
   ```

   occurrences ను కనుగొనండి.

   తర్వాత:

   - Regex ను "break" చేయడానికి ప్రయత్నించండి
   - అనూహ్య formatting లేదా code styles ను ఉపయోగించండి

   ఇప్పుడు [semgrep](#linting) ను ఉపయోగించి చూడండి.

   మీ `grep` pattern విఫలమైనా, semgrep ప్రమాదకరమైన code ను ఇంకా గుర్తించగలుగుతుందా?

5. మీ IDE లేదా Text Editor లో Regex Search-and-Replace అభ్యాసం చేయండి.

   ఈ lecture notes లోని:

   ```markdown
   -
   ```

   Markdown Bullet Markers ను:

   ```markdown
   *
   ```

   గా మార్చండి.

   గమనిక:

   File లోని అన్ని `-` characters ను blind replace చేయడం సరైంది కాదు.

   ఎందుకంటే:

   - Dates
   - Hyphenated Words
   - Command Options
   - URLs

   వంటి అనేక చోట్ల `-` వేరే అర్థంలో ఉపయోగించబడుతుంది.

   కాబట్టి, నిజమైన Markdown Bullet Markers ను మాత్రమే గుర్తించే Regex ను రూపొందించండి.

6. ఈ JSON structure నుండి:

   ```json
   {"name": "Alyssa P. Hacker", "college": "MIT"}
   ```

   `name` value ను capture చేసే Regex ను రాయండి.

   ఉదాహరణకు:

   ```text
   Alyssa P. Hacker
   ```

   ను extract చేయాలి.

   **సూచన:**

   మొదటి ప్రయత్నంలో మీరు ఇలా పొరపాటు చేయవచ్చు:

   ```text
   Alyssa P. Hacker", "college": "MIT
   ```

   వంటి ఫలితం రావచ్చు.

   దీనిని సరిచేయడానికి Python Regex Documentation లోని **Greedy Quantifiers** గురించి చదవండి.

   ### 6.1

   Name లో `"` character ఉన్న సందర్భాలను కూడా handle చేసేలా Regex ను మెరుగుపరచండి.

   JSON లో double quotes ను ఇలా escape చేయవచ్చు:

   ```json
   \" 
   ```

   కాబట్టి Regex ఈ పరిస్థితిని కూడా సరిగ్గా handle చేయాలి.

   ### 6.2

   వాస్తవ ప్రపంచంలో (practical software development లో), క్లిష్టమైన parsing సమస్యలకు Regex ఉపయోగించడం **సిఫారసు చేయబడదు**.

   ఈ task ను మీ programming language లోని JSON Parser ఉపయోగించి చేయడం నేర్చుకోండి.

   ఒక చిన్న Command-Line Program రాయండి:

   - Input: `stdin` ద్వారా JSON
   - Output: `stdout` ద్వారా name value

   ఇది చాలా చిన్న program అవుతుంది.

   Python లో అయితే:

   ```python
   import json
   ```

   తర్వాత కేవలం ఒక line code తోనే ఈ పని చేయవచ్చు.
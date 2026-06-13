---
layout: page
title: మీ CS విద్యలో మిస్సింగ్ సెమెస్టర్
description: >
  మిమ్మల్ని మరింత ఉత్పాదకమైన కంప్యూటర్ శాస్త్రవేత్తగా మరియు ప్రోగ్రామర్‌గా మార్చే శక్తివంతమైన టూల్స్ నేర్చుకోండి.
subtitle: "2026"
nositetitle: true
---

కంప్యూటర్ సైన్స్‌లోని ఉన్నత విషయాలను—ఆపరేటింగ్ సిస్టమ్స్ నుంచి మెషిన్ లెర్నింగ్ వరకు—క్లాసులు మీకు నేర్పిస్తాయి. కానీ చాలా అరుదుగా బోధించబడే, విద్యార్థులు తమంతట వారు తెలుసుకోవాల్సిన ఒక కీలక అంశం ఉంది: వారి టూల్స్‌ను సమర్థంగా ఉపయోగించడం. మేము మీకు కమాండ్-లైన్‌లో నైపుణ్యం సాధించడం, శక్తివంతమైన టెక్స్ట్ ఎడిటర్‌ను ఉపయోగించడం, వెర్షన్ కంట్రోల్ సిస్టమ్స్‌లోని అధునాతన ఫీచర్లను వినియోగించడం, ఇంకా మరెన్నో నేర్పుతాము.

విద్యార్థులు తమ విద్యా ప్రయాణంలో ఈ టూల్స్‌తో వందల గంటలు, కెరీర్ అంతటా వేల గంటలు గడుపుతారు. కాబట్టి అనుభవాన్ని ఎంతగానో సులభంగా, అంతరాయం లేకుండా చేయడం సమంజసం. ఈ టూల్స్‌లో నైపుణ్యం సాధించడం వల్ల మీ టూల్స్‌ను మీ ఇష్టానికి అనుగుణంగా మలచుకోవడానికి తక్కువ సమయం ఖర్చవుతుంది; అంతేకాదు, ఇంతకు ముందు అసాధ్యంగా అనిపించిన సమస్యలను కూడా పరిష్కరించగలుగుతారు.

ఇప్పటి కాలంలో, AI-సক্ষমమైన మరియు AI-వృద్ధి చేసిన టూల్స్, వర్క్‌ఫ్లోల ప్రవేశంతో సాఫ్ట్‌వేర్ ఇంజినీరింగ్‌లో అనేక అంశాలు వేగంగా మారుతున్నాయి. వీటిని సరిగ్గా, వాటి పరిమితులను అర్థం చేసుకుని వాడినప్పుడు, ఇవి కంప్యూటర్ సైన్స్ ప్రాక్టీషనర్లకు గణనీయమైన ప్రయోజనాలను అందిస్తాయి; అందుకే వీటి గురించి ప్రాథమిక అవగాహన పెంపొందించుకోవడం ఉపయోగకరం. AI ఒక క్రాస్-ఫంక్షనల్ ఎనేబ్లింగ్ టెక్నాలజీ కాబట్టి, ప్రత్యేక AI లెక్చర్ లేదు; బదులుగా తాజా సంబంధిత AI టూల్స్ మరియు పద్ధతులను ప్రతి లెక్చర్‌లోనే కలిపాము.

ఈ క్లాస్ వెనుక ఉన్న ప్రేరణ గురించి [ఇక్కడ చదవండి](/about/).

# సిలబస్

<ul>
{% assign lectures = site['2026'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-m/%-d/%y' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[క్లాస్ లేదు]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

## గత సంవత్సరాల ప్రత్యేక విషయాలు

మేము కవర్ చేసే విషయాలు సంవత్సరానికి సంవత్సరం మారుతాయి. సంవత్సరాలుగా బోధించిన పూర్తి విషయాల సమాహారాన్ని చూడాలనుకునే విద్యార్థుల కోసం, 2026లో కవర్ చేయని గత సంవత్సరాల అంశాలను కూడా ఇక్కడ హైలైట్ చేశాము.

{% assign sorted_collections = site.collections | sort: 'label' | pop | reverse %}
<ul>
{% for collection in sorted_collections %}
    {% assign grouped_lectures = site[collection.label] | group_by: 'date' | sort: 'name' %}
    {% for group in grouped_lectures %}
        {% assign sorted_lectures = group.items | sort: 'order' %}
        {% for lecture in sorted_lectures %}
            {% if lecture.special == true %}
                <li>
                    <strong>{{ lecture.date | date: '%-m/%-d/%y' }}</strong>:
                    <a href="{{ lecture.url }}">{{ lecture.title }}</a>
                </li>
            {% endif %}
        {% endfor %}
    {% endfor %}
{% endfor %}
</ul>

# సాధారణ సమాచారం

**సిబ్బంది**: ఈ క్లాస్‌ను Anish, Jon, మరియు Jose కలిసి బోధిస్తున్నారు.<br>
**ప్రశ్నలు**: [missing-semester@mit.edu](mailto:missing-semester@mit.edu) కి ఇమెయిల్ చేయండి.<br>
**చర్చ**: [OSSU Discord](https://ossu.dev/#community) (Piazza లాగా `#missing-semester-forum` ను వాడండి, మరియు క్లాస్/ఇన్‌స్ట్రక్టర్లతో మాట్లాడేందుకు `#missing-semester` ను వాడండి)।

# MIT కి మించి

ఈ వనరుల నుంచి మరికొందరు ప్రయోజనం పొందాలని ఆశించి, ఈ క్లాస్‌ను MIT కి బయట కూడా పంచుకున్నాము. దాని గురించి పోస్ట్‌లు మరియు చర్చలను ఇక్కడ చూడవచ్చు:

 - Hacker News ([2026](https://news.ycombinator.com/item?id=47124171), [2020](https://news.ycombinator.com/item?id=22226380), [2019](https://news.ycombinator.com/item?id=19078281))
 - Lobsters ([2026](https://lobste.rs/s/q4ykw7/missing_semester_your_cs_education_2026), [2020](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit), [2019](https://lobste.rs/s/h6157x/mit_hacker_tools_lecture_series_on))
 - r/learnprogramming ([2026](https://www.reddit.com/r/learnprogramming/comments/1r93yk6/the_missing_semester_of_your_cs_education_2026/), [2020](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/), [2019](https://www.reddit.com/r/learnprogramming/comments/an42uu/mit_hacker_tools_a_lecture_series_on_programmer/))
 - r/programming ([2020](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/), [2019](https://www.reddit.com/r/programming/comments/an3xki/mit_hacker_tools_a_lecture_series_on_programmer/))
 - X ([2026](https://x.com/anishathalye/status/2024521145777848588), [2020](https://twitter.com/jonhoo/status/1224383452591509507), [2019](https://x.com/jonhoo/status/1090323977766137858))
 - Bluesky ([2026](https://bsky.app/profile/jonhoo.eu/post/3mfa2bhyuj22i))
 - Mastodon ([2026](https://fosstodon.org/@jonhoo/116098318361854057))
 - LinkedIn ([2026](https://www.linkedin.com/posts/anishathalye_i-returned-to-mit-during-iap-january-term-activity-7430285026933522433-Ehr9))
 - YouTube ([2026](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L), [2020](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J), [2019](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuiujH1lpn8cA9dsyulbYRv))

# అనువాదాలు

{% comment %} keep these in alphabetical order {% endcomment %}

- [Arabic](https://missing-semester-ar.github.io/)
- [Bengali](https://missing-semester-bn.github.io/)
- [Chinese (Simplified)](https://missing-semester-cn.github.io/)
- [Chinese (Traditional, Taiwan)](https://missing-semester-tw.github.io/)
- [German](https://missing-semester-de.github.io/)
- [Hindi](https://missing-semester-hi.github.io/)
- [Italian](https://missing-semester-it.github.io/)
- [Japanese](https://missing-semester-jp.github.io/)
- [Kannada](https://missing-semester-kn.github.io/)
- [Korean](https://missing-semester-kr.github.io/)
- [Persian](https://missing-semester-fa.github.io/)
- [Portuguese](https://missing-semester-pt.github.io/)
- [Russian](https://missing-semester-rus.github.io/)
- [Serbian](https://netboxify.com/missing-semester/)
- [Spanish](https://missing-semester-esp.github.io/)
- [Swedish](https://den-saknade-terminen.l10n.se/)
- [Telugu](https://missing-semester-te.github.io/)
- [Thai](https://missing-semester-th.github.io/)
- [Turkish](https://missing-semester-tr.github.io/)
- [Vietnamese](https://missing-semester-vn.github.io/)

గమనిక: ఇవి సముదాయ అనువాదాలకు చెందిన బాహ్య లింకులు. మేము వీటిని ధృవీకరించలేదు。

ఈ తరగతి నోట్స్‌కు మీరు అనువాదం చేశారా? జాబితాలో చేర్చేందుకు ఒక [pull request](https://github.com/missing-semester/missing-semester/pulls) పంపండి。
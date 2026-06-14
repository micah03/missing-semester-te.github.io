---
layout: page
title: "2020 ఉపన్యాసాలు"
description: >
  Missing Semester, MIT IAP 2020 కోసం ఉపన్యాస గమనికలు మరియు వీడియోలు.
permalink: /te/2020/
phony: true
---

<ul class="double-spaced">
  {% assign lectures = site['2020'] | sort: 'date' %}
  {% for lecture in lectures %}
    {% if lecture.phony != true %}
      <li>
        <strong>{{ lecture.date | date: '%-m/%-d' }}</strong>:
        {% if lecture.ready %}
          <a href="https://missing.csail.mit.edu{{ lecture.url }}">
  {{ lecture.title }} (English)
</a>
        {% elsif lecture.noclass %}
          {{ lecture.title }} [క్లాస్ లేదు]
        {% else %}
          {{ lecture.title }} [త్వరలో వస్తుంది]
        {% endif %}
        {% if lecture.details %}
          <br>
          ({{ lecture.details }})
        {% endif %}
      </li>
    {% endif %}
  {% endfor %}
</ul>

ఉపన్యాసాల వీడియో రికార్డింగులు [YouTube లో అందుబాటులో ఉన్నాయి](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J)।

# MIT కి మించి

ఈ వనరుల నుంచి మరికొందరు ప్రయోజనం పొందాలని ఆశించి, ఈ క్లాస్‌ను MIT కి బయట కూడా పంచుకున్నాము. దాని గురించి పోస్ట్‌లు మరియు చర్చలను ఇక్కడ చూడవచ్చు:

 - [Hacker News](https://news.ycombinator.com/item?id=22226380)
 - [Lobsters](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit)
 - [r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/)
 - [r/programming](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/)
 - [Twitter](https://twitter.com/jonhoo/status/1224383452591509507)
 - [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J)

# కృతజ్ఞతలు

వీడియో ఉపన్యాసాలను రికార్డ్ చేయడం సాధ్యమయ్యేలా చేసిన Elaine Mello, Jim Cain, మరియు [MIT Open Learning](https://openlearning.mit.edu/) కు; A/V పరికరాల కోసం Anthony Zolnik మరియు [MIT AeroAstro](https://aeroastro.mit.edu/) కు; అలాగే ఈ క్లాస్‌కు మద్దతు ఇచ్చిన Brandi Adams మరియు [MIT EECS](https://www.eecs.mit.edu/) కు మేము కృతజ్ఞతలు తెలుపుతున్నాం.
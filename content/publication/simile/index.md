---
title: 'Can Pre-trained Language Models Interpret Similes as Smart as Human?'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - Qianyu He
  - admin
  - Zhixu Li
  - Rui Xie
  - Yanghua Xiao

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

date: '2022-05-22T00:00:00Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2022-05-22T00:00:00Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['1']

# Publication name and optional abbreviated publication name.
publication: In *Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics*
publication_short: In *ACL 2022*

abstract: Simile interpretation is a crucial task in natural language processing. Nowadays, pre-trained language models (PLMs) have achieved stateof-the-art performance on many tasks. However, it remains under-explored whether PLMs can interpret similes or not. In this paper, we investigate the ability of PLMs in simile interpretation by designing a novel task named Simile Property Probing, i.e., to let the PLMs infer the shared properties of similes. We construct our simile property probing datasets from both general textual corpora and humandesigned questions, containing 1,633 examples covering seven main categories. Our empirical study based on the constructed datasets shows that PLMs can infer similes’ shared properties while still underperforming humans. To bridge the gap with human performance, we additionally design a knowledge-enhanced training objective by incorporating the simile knowledge into PLMs via knowledge embedding methods. Our method results in a gain of 8.58% in the probing task and 1.37% in the downstream task of sentiment classification.

# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags: [Foundation Models]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://aclanthology.org/2022.acl-long.543.pdf'
url_code: 'https://github.com/Abbey4799/PLMs-Interpret-Simile'
# url_dataset: ''
# url_poster: ''
# url_project: ''
# url_slides: ''
# url_source: ''
# url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: ''
  focal_point: ''
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects:
#   - example

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
# slides: example
---

<!-- {{% callout note %}}
Click the _Cite_ button above to demo the feature to enable visitors to import publication metadata into their reference management software.
{{% /callout %}}

{{% callout note %}}
Create your slides in Markdown - click the _Slides_ button to check out the example.
{{% /callout %}}
 -->
<!-- Supplementary notes can be added here, including [code, math, and images](https://wowchemy.com/docs/writing-markdown-latex/). -->

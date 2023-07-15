---
title: 'Modeling Adversarial Attack on Pre-trained Language Models as Sequential Decision Making'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - Xuanjie Fang
  - admin
  - Yang Liu
  - Wei Wang

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

date: '2023-07-01T00:00:00Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2023-07-01T00:00:00Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['1']

# Publication name and optional abbreviated publication name.
publication: In *Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics*
publication_short: In *ACL 2023*

abstract: Pre-trained language models (PLMs) have been widely used to underpin various downstream tasks. However, the adversarial attack task has found that PLMs are vulnerable to small perturbations. Mainstream methods adopt a detached two-stage framework to attack without considering the subsequent influence of substitution at each step.In this paper, we formally model the adversarial attack task on PLMs as a sequential decision-making problem, where the whole attack process is sequential with two decision-making problems, i.e., word finder and word substitution. Considering the attack process can only receive the final state without any direct intermediate signals, we propose to use reinforcement learning to find an appropriate sequential attack path to generate adversaries, named SDM-ATTACK. Our experimental results show that SDM-ATTACK achieves the highest attack success rate with a comparable modification rate and semantic similarity to attack fine-tuned BERT. Furthermore, our analyses demonstrate the generalization and transferability of SDM-ATTACK.Resources of this work will be released after this paper’s publication.
# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags: [Adversarial]

# Display this page in the Featured widget?
featured: false

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://aclanthology.org/2023.findings-acl.461.pdf'
# url_code: ''
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

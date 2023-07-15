---
title: 'Prompt-Guided Retrieval Augmentation for Non-Knowledge-Intensive Tasks'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - Zhicheng Guo
  - admin
  - Yile Wang
  - Peng Li
  - Yang Liu

# Author notes (optional)
# author_notes:
#   - 'Equal contribution'
#   - 'Equal contribution'

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
publication: In *Findings Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics*
publication_short: In *ACL 2023*

abstract: Retrieval-augmented methods have received increasing attention to support downstream tasks by leveraging useful information from external resources. Recent studies mainly focus on exploring retrieval to solve knowledge-intensive (KI) tasks. However, the potential of retrieval for most non-knowledge-intensive (NKI) tasks remains under-explored. There are two main challenges to leveraging retrieval-augmented methods for NKI tasks. 1) the demand for diverse relevance score functions and 2) the dilemma between training cost and task performance. To address these challenges, we propose a two-stage framework for NKI tasks, named PGRA. In the first stage, we adopt a task-agnostic retriever to build a shared static index and select candidate evidence efficiently. In the second stage, we design a prompt-guided reranker to rerank the nearest evidence according to task-specific relevance for the reader. Experimental results show that PGRA outperforms other state-of-the-art retrieval-augmented methods. Our analyses further investigate the influence factors to model performance and demonstrate the generality of PGRA. The code and model will be released for further research.

# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags: [Foundation Models]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://aclanthology.org/2023.findings-acl.693.pdf'
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

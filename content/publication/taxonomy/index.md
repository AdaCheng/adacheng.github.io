---
title: 'Learning What You Need from What You Did: Product Taxonomy Expansion with User Behaviors Supervision'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin
  - Zhouhong Gu
  - Bang Liu
  - Rui Xie
  - Wei Wu
  - Yanghua Xiao

# Author notes (optional)
# author_notes:
#   - 'Equal contribution'
#   - 'Equal contribution'

date: '2022-05-09T00:00:00Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2022-05-09T00:00:00Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['1']

# Publication name and optional abbreviated publication name.
publication: In *38th IEEE International Conference on Data Engineering*
publication_short: In *ICDE 2022*

abstract: —Taxonomies have been widely used in various domains to underpin numerous applications. Specially, product taxonomies serve an essential role in the e-commerce domain for the recommendation, browsing, and query understanding. However, taxonomies need to constantly capture the newly emerged terms or concepts in e-commerce platforms to keep up-to-date, which is expensive and labor-intensive if it relies on manual maintenance and updates. Therefore, we target the taxonomy expansion task to attach new concepts to existing taxonomies automatically. In this paper, we present a self-supervised and user behavior-oriented product taxonomy expansion framework to append new concepts into existing taxonomies. Our framework extracts hyponymy relations that conform to users’ intentions and cognition. Specifically, i) to fully exploit user behavioral information, we extract candidate hyponymy relations that match user interests from query-click concepts; ii) to enhance the semantic information of new concepts and better detect hyponymy relations, we model concepts and relations through both usergenerated content and  tructural information in existing taxonomies and user click logs, by leveraging Pre-trained Language Models and Graph Neural Network combined with Contrastive Learning; iii) to reduce the cost of dataset construction and overcome data skews, we construct a high-quality and balanced training dataset from existing taxonomy with no supervision. Extensive experiments on real-world product taxonomies in Meituan Platform, a leading Chinese vertical e-commerce platform to order take-out with more than 70 million daily active users, demonstrate the superiority of our proposed framework over state-of-the-art methods. Notably, our method enlarges the size of real-world product taxonomies from 39,263 to 94,698 relations with 88% precision.

# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags: [Taxonomy]

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9835349'
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

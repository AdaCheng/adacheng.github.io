---
title: "Federated Learning of Larger Server Models via Selective Knowledge Fusion"
authors:
- admin
- Jingwen Wu
- Yanghua Xiao
- Yang Liu
- Yang Liu
date: "2021-10-21T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2021-10-21T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["3"]

# Publication name and optional abbreviated publication name.
publication: ""
publication_short: ""

abstract: Today data is often scattered among billions of resource-constrained edge devices with security and privacy constraints. Federated Learning (FL) has emerged as a viable solution to learn a global model while keeping data private, but the model complexity of FL is impeded by the computation resources of edge nodes. In this work, we investigate a novel paradigm to take advantage of a powerful server model to break through model capacity in FL. By selectively learning from multiple teacher clients and itself, a server model develops in-depth knowledge and transfers its knowledge back to clients in return to boost their respective performance. Our proposed framework achieves superior performance on both server and client models and provides several advantages in a unified framework, including flexibility for heterogeneous client architectures, robustness to poisoning attacks, and communication efficiency between clients and server on various image classification tasks.
# Summary. An optional shortened abstract.
summary: We propose a novel paradigm in federated learning where a powerful server model learns from multiple teacher clients and transfers knowledge back to boost client performance, addressing the limitations of model complexity on edge devices. Our framework achieves superior performance on image classification tasks, providing flexibility for heterogeneous client architectures, robustness against attacks, and efficient communication between clients and the server.
tags:
- Federated Learning
featured: true

# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: https://arxiv.org/abs/2110.11027
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
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects: [] 

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---

{{% callout note %}}
Create your slides in Markdown - click the *Slides* button to check out the example.
{{% /callout %}}

Supplementary notes can be added here, including [code, math, and images](https://wowchemy.com/docs/writing-markdown-latex/).

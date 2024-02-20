---
# Leave the homepage title empty to use the site title
title:
date: 2022-10-24
type: landing

sections:
  - block: about.biography
    id: about
    content:
      title: Biography
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
#  - block: features
#    content:
#      title: Skills
#      items:
#        - name: R
#          description: 90%
#          icon: r-project
#          icon_pack: fab
#        - name: Statistics
#          description: 100%
#          icon: chart-line
#          icon_pack: fas
  - block: markdown
    id: news
    content:
      title: News
      text: <ul><li><strong>Jan. 2024:</strong> Our paper <a href="https://arxiv.org/abs/2309.11235">OpenChat&#58; Advancing Open-source Language Models with Mixed-Quality Data</a> has been accepted by ICLR 2024!</li>       <li><strong>Nov. 2023:</strong> Our paper <a href="https://arxiv.org/abs/2311.15596">Can Vision-Language Models Think from a First-Person Perspective?</a> and its corresponding benchmark <a href="https://adacheng.github.io/EgoThink/">EgoThink</a> are released!</li>            <li><strong>Sep. 2023:</strong> Our paper <a href="https://arxiv.org/abs/2305.17650">Evolving Connectivity for Recurrent Spiking Neural Networks</a> has been accepted by NeurIPS 2023!</li>      <li><strong>Jul. 2023:</strong> Our paper <a href="https://arxiv.org/abs/2307.02752">Offline Reinforcement Learning with Imbalanced Datasets</a> has been accepted by ICML Workshop 2023!</li>      <li><strong>May 2023:</strong> Three papers have been accepted by ACL 2023!</li>      <li><strong>Nov. 2022:</strong> Our paper <a href="https://ojs.aaai.org/index.php/AAAI/article/view/26494">Unsupervised Explanation Generation via Correct Instantiations</a> has been accepted by AAAI 2023!</li>      <li><strong>Oct. 2022:</strong> I was awarded with China National Scholarship!</li>

    design:
      columns: '2' 
  - block: experience
    id: experience
    content:
      title: Experience
      # Date format for experience
      #   Refer to https://wowchemy.com/docs/customization/#date-format
      date_format: Jan 2006
      # Experiences.
      #   Add/remove as many `experience` items below as you like.
      #   Required fields are `title`, `company`, and `date_start`.
      #   Leave `date_end` empty if it's your current employer.
      #   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
      items:
        - title: Research Intern
          company: Pre-train and Multi-modal Group, 01.AI
          company_url: 'https://01.ai/'
          company_logo: org-01
          location: Beijing, China
          date_start: '2023-08-01'
          date_end: ''
          description: Supervised Fine-tuinng, working with Wenhao Huang, Xiang Yue and Xiangang Li.
        - title: Investment Intern
          company: Investment Department, Sinovation Ventures
          company_url: 'https://www.chuangxin.com/'
          company_logo: org-cx
          location: Beijing, China
          date_start: '2023-02-01'
          date_end: ''
          description: Investigation for Artificial Intelligence, working with Bobing Ren.
        - title: Research Intern
          company: Natural Language Processing Group, Pujiang Lab
          company_url: 'https://www.shlab.org.cn/aboutus'
          company_logo: org-pjlab
          location: Shanghai, China
          date_start: '2022-03-01'
          date_end: '2022-12-01'
          description: Free-text explanation for reasoning, working with Prof. [Lingpeng Kong](https://ikekonglp.github.io/).
        - title: Visiting Fellow
          company: Institue for AI Industry Research, Tsinghua University
          company_url: 'https://air.tsinghua.edu.cn/'
          company_logo: org-thu
          location: Beijing, China
          date_start: '2021-07-01'
          date_end: '2022-02-01'
          description: Large models as continual knowledge bases, advised by Prof. [Yang Liu](https://nlp.csai.tsinghua.edu.cn/~ly/) and Prof. [Yang Liu](https://sites.google.com/site/yangliuveronica/).
        - title: Research Intern
          company: Natural Language Understanding Group, Meituan NLP Center
          company_url: 'https://about.meituan.com/home' 
          company_logo: org-meituan
          location: Beijing, China
          date_start: '2020-11-01'
          date_end: '2021-06-01'
          description: Taxonomy relational knowledge extraction, working with [Rui Xie](https://scholar.google.com/citations?hl=en&user=_GaB4AQAAAAJ). -> **ICDE 2022** and **ACL 2022**
        - title: Visiting Fellow
          company: Text Intelligence Lab, Westlake University
          company_url: 'https://www.westlake.edu.cn/' 
          company_logo: org-westlake
          location: Hangzhou, China
          date_start: '2019-09-01'
          date_end: '2020-09-01'
          description: Analysis of commonsense knowledge in pre-trained language model, advised by Prof. [Yue Zhang](https://frcchang.github.io/). -> **ACL 2021**
    design:
      columns: '2'
#  - block: collection
#    id: posts
#    content:
#      title: Recent Posts
#      subtitle: ''
#      text: ''
#      # Choose how many pages you would like to display (0 = all pages)
#      count: 5
#      # Filter on criteria
#      filters:
#        folders:
#        - post
#        author: ""
#        category: ""
#        tag: ""
#        exclude_featured: false
#        exclude_future: false
#        exclude_past: false
#        publication_type: ""
      # Choose how many pages you would like to offset by
#      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
#      order: desc
#    design:
      # Choose a layout view
#      view: compact
#      columns: '2'
#  - block: markdown
#    content:
#      title: Gallery
#      subtitle: ''
#      text: |-
#        {{< gallery album="demo" >}}
#    design:
#      columns: '1'
  - block: collection
    id: featured
    content:
      title: Featured Publications
      filters:
        folders:
          - publication
        featured_only: true
    design:
      columns: '1'
      view: compact
  - block: collection
    content:
      title: Recent Publications
      text: |-
        {{% callout note %}}
        Quickly discover relevant content by [filtering publications](./publication/).
        {{% /callout %}}
      filters:
        folders:
          - publication
        exclude_featured: true
    design:
      columns: '2'
      view: citation
  - block: portfolio
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - project
      # Default filter index (e.g. 0 corresponds to the first `filter_button` instance below).
      default_button_index: 0
      # Filter toolbar (optional).
      # Add or remove as many filters (`filter_button` instances) as you like.
      # To show all items, set `tag` to "*".
      # To filter by a specific tag, set `tag` to an existing tag name.
      # To remove the toolbar, delete the entire `filter_button` block.
#      buttons:
#        - name: All
#          tag: '*'
#        - name: Deep Learning
#          tag: Deep Learning
#        - name: Other
#          tag: Demo
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '1'
      view: showcase
      # For Showcase view, flip alternate rows?
      flip_alt_rows: false
  - block: accomplishments
    id: awards
    content:
      # Note: `&shy;` is used to add a 'soft' hyphen in a long heading.
      title: 'Awards'
      subtitle:
      # Date format: https://wowchemy.com/docs/customization/#date-format
      date_format: Jan 2006
      # Accomplishments.
      #   Add/remove as many `item` blocks below as you like.
      #   `title`, `organization`, and `date_start` are the required parameters.
      #   Leave other parameters empty if not required.
      #   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
      items:
        - certificate_url: ''
          date_end: ''
          date_start: '2023-03-29'
          description: ''
          organization: Shanghai
          organization_url: ''
          title: Excellent Graduates
          url: ''
        - certificate_url: ''
          date_end: ''
          date_start: '2022-10-08'
          description: ''
          organization: China
          organization_url: ''
          title: National Scholarship
          url: ''
        - certificate_url: ''
          date_end: ''
          date_start: '2020-12-05'
          description: ''
          organization: Fudan University
          organization_url: ''
          title: 1st Place in Women’s Basketball Graduate School Cup
          url: ''
    design:
      columns: '2'
#  - block: collection
#    id: talks
#    content:
#      title: Recent & Upcoming Talks
#      filters:
#        folders:
#          - event
#    design:
#      columns: '2'
#      view: compact
#  - block: tag_cloud
#    content:
#      title: Popular Topics
#    design:
#      columns: '2'
---

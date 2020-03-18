---
title: Home
sidebar:
  entries:
    - title: Welcome
      url: '#intro'
      is_primary: true
    - title: What we do
      url: '#one'
      is_primary: false
    - title: How we do
      url: '#two'
      is_primary: false
    - title: Get in touch
      url: '#three'
      is_primary: false
sections:
  - type: intro
    title: superacazzole
    subtitle: >-
      We are building an Open and Welcoming Technology & AI Community. 


      Follow supercazzole  and help us to grow in AI Experiments, Made in Italy
      of course!
    section_id: intro
    background_style: style1
    actions:
      - label: Learn more
        url: '#one'
        is_scrolly: true
        is_primary: false
    component: intro.html
  - type: spotlights
    title: Spotlights Section
    section_id: one
    background_style: style2
    component: spotlights.html
  - type: features
    title: How we do
    subtitle: >-
      We use a lot of Free Tools provided by the Open Source Community or
      fantastic Internet Companies. 

      This is just a small list to share with you these fantastic products and
      services. 
    section_id: two
    background_style: style3
    features_list:
      - title: 'JamStack '
        text: >-
          Fast and secure sites and apps delivered by pre-rendering files and
          serving them directly from a CDN, removing the requirement to manage
          or run web servers.
        icon: fa-code
      - title: Github
        text: >-
          GitHub is a Git repository hosting service, but it adds many of its
          own features. While Git is a command line tool, GitHub provides a
          Web-based graphical interface. It also provides access control and
          several collaboration features, such as a wikis and basic task
          management tools for every project
        icon: fa-github
      - title: Cloudflare
        text: >-
          Cloudflare, Inc. is an American web-infrastructure and
          website-security company, providing content-delivery-network services,
          DDoS mitigation, Internet security, and distributed domain-name-server
          services
        icon: fa-cloud
      - title: 'Netlify '
        text: >-
          Used by more than 800,000 web developers and businesses, the Netlify
          platform provides modern build workflows, serverless functions and a
          global Application Delivery Network to deliver the most performant,
          secure and scalable websites and applications.
        icon: fa-server
      - title: 'Python '
        text: >-
          Python is an interpreted, high-level, general-purpose programming
          language. Created by Guido van Rossum and first released in 1991,
          Python's design philosophy emphasizes code readability with its
          notable use of significant whitespace.
        icon: fa-code
      - title: Jupyter Notebook
        text: >-
          The Jupyter Notebook is an open-source web application that allows you
          to create and share documents that contain live code, equations,
          visualizations and narrative text. Uses include: data cleaning and
          transformation, numerical simulation, statistical modeling, data
          visualization, machine learning, and much more.
        icon: fa-clipboard
    actions:
      - label: Learn more
        url: /generic
        is_scrolly: false
        is_primary: false
    component: features.html
  - type: contact
    title: Get in touch
    text: >-
      contact me if you need information or want to make a report on a website content, remember that this is a fan site so it may take several days before you can receive a reply.
    section_id: three
    background_style: style1
    contact_list:
      - title: Address
        text: |-
          Rome, 00100
          ITALY
      - title: Email
        text: adrianoamalfi@gmail.com
        url: '#'
    social:
      title: Social
      social_icons:
        - title: Twitter
          icon: fa-twitter
          url: 'https://twitter.com/adrianoamalfi/'
        - title: GitHub
          icon: fa-github
          url: 'https://github.com/adrianoamalfi/'
        - title: LinkedIn
          icon: fa-linkedin
          url: 'https://www.linkedin.com/in/adrianoamalfi/'
    component: contact.html
menus:
  main:
    title: Home
    weight: 1
layout: home
---

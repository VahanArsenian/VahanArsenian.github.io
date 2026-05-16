---
title: ""
summary: ""
date: 2026-04-20
type: landing

design:
  spacing: "4rem"

sections:
  - block: resume-biography-3
    id: about
    content:
      username: admin
      text: ""
      headings:
        about: "About"
    design:
      background:
        gradient_mesh:
          enable: true
      name:
        size: sm
      avatar:
        size: medium
        shape: circle

  - block: announcements
    id: announcements
    content:
      title: "Announcements"
      icon: "megaphone"
      cutoff_years: 2
    design:
      columns: "1"

  - block: collection
    id: publications
    content:
      title: "Recent Publications"
      icon: "document-text"
      text: ""
      filters:
        folders:
          - publication
        recent_years: 2
    design:
      view: citation

  - block: interests
    id: interests
    content:
      username: admin
      title: "Research Interests"

  - block: markdown
    id: hobbies-preview
    content:
      title: "Hobbies"
      icon: "puzzle-piece"
      text: |-
        Things I spend non-research hours on — games I've sunk real time
        into, anime I keep thinking about.

        [Browse the hobbies hub →](/hobbies/)
    design:
      columns: "1"

  - block: education
    id: education
    content:
      username: admin
      title: "Education"
---

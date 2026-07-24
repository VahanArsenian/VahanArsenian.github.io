---
title: "banditry: an online learning framework with optimism-in-the-face-of-uncertainty and posterior-sampling agents"
authors:
  - admin
date: "2026-07-21"

publishDate: "2026-07-24"

publication_types: ["software"]

publication: "Open-source Python library (MIT), PyPI: `banditry`"
publication_short: ""

abstract: >-
  banditry provides contextual bandit agents for black-box optimisation over
  mixed design spaces. It runs a suggest → evaluate → observe loop against an
  expensive black-box objective and exposes the pieces of that loop as
  composable modules: a GP-UCB / OFU agent with Bayesian or frequentist
  (Chowdhury–Gopalan) confidence widths optimising the MACE acquisition
  ensemble, and a Thompson-sampling agent whose neural value function is
  sampled by MCMC oracles (Langevin dynamics or NUTS), with optional Feel-Good
  reweighting. Surrogates include exact and sparse variational Gaussian
  processes; acquisition optimisation wraps evolutionary (NSGA-II/III) and
  SGLD oracles. Design spaces mix numeric, integer, boolean, and categorical
  parameters, and any subset of parameters can be pinned to observed context
  each round, turning the loop into a contextual bandit.

summary: >-
  A Python library of contextual bandit agents (GP-UCB, Thompson sampling)
  for black-box optimisation over mixed design spaces.

tags:
  - Bandits
  - Bayesian Optimization
  - Online Learning
  - Software

featured: false

doi: "10.5281/zenodo.21499195"

links:
  - type: code
    url: https://github.com/VahanArsenian/banditry
  - label: Docs
    url: https://vahanarsenian.github.io/banditry/
  - label: PyPI
    url: https://pypi.org/project/banditry/

image:
  caption: ""
  focal_point: ""
  preview_only: false

projects: []
slides: ""
---

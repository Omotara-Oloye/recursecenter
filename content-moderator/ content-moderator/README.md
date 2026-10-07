# Omotara's Content Moderator

A small browser game about content moderation. You review a queue of 20 made-up social media posts and decide whether to keep or remove each one. Two automated helpers are working alongside you, and at the end you see how all three of you did.

**Play it:** [(https://omotara-oloye.github.io/recursecenter/content-moderator/)]

I'm exploring AI trust and safety during my batch, and this was what I decided to work on during Impossible Day.

## How it works

- **Keep or remove.** Each post comes with a verdict from two helpers. After a wrong answer, a popup explains why.
- **Keyword filter.** Hand-written rules that flag posts containing certain words, like "free", "click" and "invest".
- **Learned model.** A small classifier that learns from your answers as you play. It says "still learning" until it has seen 5 posts.
- **Two rounds.** The first 14 posts are plain and come in random order. The last 6 are disguised versions: letters swapped for numbers, dots inside words, trigger words removed. They are also shuffled.
- **Results.** Your mistakes with explanations, a scoreboard comparing you, the keyword filter and the model, and sections showing where the filter went wrong and what the model learned.

## What it's meant to show

- Hand-written rules are predictable but easy to get around. "fr33" and "I.N.V.E.S.T" slip past a word list.
- A learned model can catch some disguises the rules miss, because it picks up patterns, like "$50 into $5,000", instead of fixed words. It also makes its own mistakes, and with only 20 examples much of what it learns is coincidence.
- Both can wrongly flag innocent posts, like the thermostat joke or the cat photos.

In my testing the keyword filter is right on 11 of the 20 posts. The model's score changes with the order of the posts, and is usually around 9 of the 15 posts it makes a guess on.

## The model

For every word, it counts how many removed posts and how many kept posts contained it. To score a new post, it starts from how often you've been removing posts, then adds the evidence from each word it has seen before. Add-one smoothing keeps a word that's missing from one side from breaking the math, and the total is turned into a probability. This is a Naive Bayes classifier, written from scratch in JavaScript with no libraries: `tokenize`, `train` and `predict`.

## Built with

Plain HTML, CSS and JavaScript in a single file. There are no frameworks, no dependencies, and no server, and nothing you do is collected or stored. Fonts load from Google Fonts and fall back to system fonts offline.

## Run it locally

Download `index.html` and open it in a browser.

## Notes

All posts are fictional examples written for this game.

## What I'd do next
I'd like to pull/scrape actual real posts from Twitter or Bluesky, add some educational piece around why content moderation differs on each social media platform, and/or add more kinds of ambiguous posts.

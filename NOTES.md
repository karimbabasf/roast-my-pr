# Notes

Written the night of the event, kept as it was rather than tidied up afterwards.

## What the scoring actually rewarded

Out of 100: working photo feature 25, **quality of Qodo usage 25**, one-to-many
addresses 20, code quality 15, UI polish 10, creativity 5, plus 10 bonus. Tie-break was
whoever merged first.

The review conversation was worth as much as the first feature. In the organisers'
words, "running /review is easy, engaging with it is the point." Applying a suggestion
scored, and so did rejecting one with a technical reason. Asking a pointed question
before the reviewer complained scored higher than fixing something silently.

## The finding that saved the demo

Qodo caught that a 2 MB photo becomes about 2.67 MB once base64 encoded, which exceeds
the 1 MB default body limit on a Next.js Server Action. Any real phone photo would have
failed with a framework error rather than a validation message, on stage.

Its suggested fix was to raise the limit. I pushed back and downscaled in the browser
instead: an avatar does not need 2 megapixels, and shrinking to 512px lands around
90 KB. Qodo agreed that was the better call, then added three refinements I had not
thought of, including preserving EXIF orientation so phone photos are not sideways.
That one change also closed a separate finding about list responses carrying tens of
megabytes of photo data.

## Arguments worth having

Twice Qodo warned that `create_all` never alters an existing table, so removing the
flat address columns would strand data. True in general. Not true here, because the
database file is built fresh and there is no deployed instance to upgrade. Adding a
migration tool to a two-hour app would have cost more than it protected.

It also flagged a seeding race between multiple workers. The app runs a single uvicorn
process with no workers argument, so the race is not reachable. Pointing at the line
settled it.

## Getting it wrong

I told Qodo the README had already been rewritten to document the new address shape. It
had been, but only in my working tree, and it never made it into the commit. Qodo was
right and I was wrong. I shipped a correction and said so in the thread. Check
`git status` before claiming something is committed.

## What I would take to the next one

- Treat the pull request page as the deliverable, not the code, when that is what is
  being judged. Budget time for the conversation from the first commit.
- Ask the reviewer about a decision you made on purpose. It finds things, and a
  question you asked reads better than a fix you made quietly.
- Splitting parallel work by repository avoids conflicts. Work that had to touch the
  same files was written to a scratch directory first and applied in one pass.
